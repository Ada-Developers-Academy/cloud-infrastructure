# Deployment Strategies & Risk Management

## Learning Goals

- Describe common deployment strategies and be able to discuss their tradeoffs around risk management and complexity. 
- Describe feature flags and where it can be useful to use configuration to decouple deployments from releasing features.

## Vocabulary and Synonyms

| Vocab | Definition | Synonyms | How to Use in a Sentence |
| --------- | --------- | -------- | --------- |
| Change Management | The process of introducing updates to a system safely. It includes planning, testing, reviewing the potential impact of a change, and having a rollback plan ready in case something goes wrong. | Release management, change control | "Before deploying the database migration, the team followed their change management process by documenting the steps, reviewing the rollback plan, and scheduling the update during a low-traffic window." |
| Feature Flagging | A way to turn features on or off without redeploying code. Useful for gradual rollouts, testing features with small groups, or quickly disabling something if it breaks in production. | Feature flags, feature toggles, feature switches | "We shipped the new dashboard behind a feature flag so we could enable it for internal testers first before rolling it out to all users." |
| Configuration Management | Managing application settings separately from code. Things like API keys, feature flags, and environment variables are stored securely and injected at runtime rather than hardcoded into the application. | Runtime configuration, config management, secrets management | "We use configuration management to store our database credentials and feature flags outside of the codebase, so we can update them without redeploying the application." |

## Evolution of Deployment Strategies

Before modern CI/CD pipelines existed, software releases were infrequent, high-stakes events. Teams spent weeks accumulating changes, testing exhaustively, and then deploying everything at once in what engineers call a big bang or all-at-once deploy. The intuition behind this approach made sense: the more you test before releasing, the safer the release should be.

In practice, big bang deploys produced a predictable set of problems that are still common in organizations that haven't updated their release procedures:
- **The blast radius is always maximum**. When a bug reaches production, it reaches every user simultaneously. There's no gradual discovery; we go from "everything is fine" to "the entire system is broken" instantly.
- **Rollback is a second high-risk deploy**. Undoing a big bang deploy means pushing the previous version to all servers at once, which is itself a deployment and can fail in its own ways.
- **Big releases accumulate big diffs**. When a month of changes ship in one deploy, diagnosing which change caused a problem can be genuinely hard. The more changes in a release, the harder it is to isolate the cause of an issue.
- **Release day becomes a recurring crisis**. Teams develop whole rituals around deploy days (code freezes, late-night maintenance windows, mandatory on-call standby) because experience has taught them to expect something to go wrong.

The engineering industry's response to these problems was to develop deployment strategies: deliberate methods for releasing changes in ways that limit exposure, preserve the ability to recover quickly, and make it easier to observe how a new version behaves before it affects every user.

Below, we'll cover four deployment strategies commonly used in enterprise software today. Each one trades something, typically speed, complexity, or cost, in exchange for different kinds of safety guarantees. Understanding the tradeoffs is what lets you choose the right tool for a given release!

## Rolling Deployment

A rolling deployment replaces the old version of an application with the new version gradually, one instance (or small group of instances) at a time, rather than replacing all instances simultaneously. At any given moment during the rollout, some servers are running the old version and some are running the new version. Incoming traffic is distributed across both versions, each server transitions to the new version as its turn comes up in the rollout sequence, and the balance shifts as the rollout progresses until all servers are running the new version. 

### Strategy Comparison

This rolling strategy helps reduce risk compared to an all-at-once deploy. As the new version is rolled out, we can monitor the updated servers for issues. If a problem emerges, the rollout can be stopped at any point before the new version reaches the full fleet.

### Benefits

- **Reduced customer impact**: At any point during the rollout, only a fraction of users are running the new version. A bug affects that fraction, not everyone.
- **No downtime**: Because instances are replaced one at a time while others continue serving traffic, the application stays available throughout the deployment.
- **Simple to implement**: Most managed container platforms and cloud orchestration tools support rolling deployments natively. It's often the default strategy for teams that haven't made a more deliberate choice.

### Tradeoffs

During a rolling deployment, our application is briefly running two versions at once. For many changes this is fine, but for changes that alter database schemas, API contracts, or message formats, both versions need to be able to coexist. If the new version writes data in a format the old version can't read, we'll have a problem mid-rollout.

| Metric | Notes |
|---|---|
| **Speed** | Slower than big bang; the rollout takes time proportional to instance count. Configurable, but you're trading speed for safety. |
| **Risk** | Lower than big bang, but not zero. If a bug only surfaces under specific load patterns, it might not appear until the rollout is nearly complete. |
| **Complexity** | Low. It's a sequential replacement of instances, and most orchestration tools manage this automatically. |
| **Observability needs** | Moderate. We need monitoring in place to notice if the new version is behaving differently from the old one while both are running. Version-tagged metrics help here. |

## Canary Releases

A canary release routes a small, controlled percentage of production traffic to the new version while the majority of users continue on the old version. The new version runs in production but is observed carefully over an extended window before any decision is made to expand the rollout. If the canary version shows elevated error rates, increased latency, or degraded business metrics compared to the control group, the release can be stopped before it affects a significant portion of users.

### Strategy Comparison

Rolling deployments are a sequential replacement strategy: every instance eventually gets the new version, and the only question is how fast. Canary releases introduce an explicit **observation phase**: the new version receives a controlled slice of traffic (say, 1–5% to start), and the team watches how it performs before deciding whether to expand the rollout. You might keep a canary running for hours or days at a small traffic percentage before promoting it. A rolling deploy doesn't have that concept of a deliberate holding pattern.

### Benefits

- **Early signal from real users.** Testing can only simulate so much. A canary running against real production traffic surfaces problems that test environments miss: edge cases in real user data, unexpected usage patterns, third-party integrations that behave differently under real load.
- **Controlled blast radius.** If the canary version has a bug, it affects only the users in the canary slice. A 5% canary means 95% of users are unaffected.
- **Data-driven promotion decisions.** Rather than relying on test results alone, you can compare error rates, latency percentiles, and business metrics between the canary and the baseline before deciding whether to continue the rollout.

### Tradeoffs

Canary releases require thoughtful user segmentation decisions. The canary slice needs to be statistically representative of your user base. If the 5% receiving the new version skews heavily toward a particular region, device type, or user cohort, the signal you get may not predict how the full rollout will behave.

| Metric | Notes |
|---|---|
| **Speed** | Slower than rolling, and significantly slower than big bang. The observation period is intentional, but it means features take longer to reach all users. |
| **Risk** | Lower than rolling for catching subtle, production-specific bugs. The deliberate observation phase is the key difference. |
| **Complexity** | Moderate to high. Traffic splitting requires routing infrastructure. Comparing canary metrics against baseline requires solid instrumentation and observability tooling. |
| **Observability needs** | High. Canary releases only provide value if you can distinguish how the canary group is performing from the control group. You need version-tagged metrics, error rates, and ideally business-level signals like conversion rates. |

## Blue/Green Deployment

In a blue/green deployment, you maintain **two complete, identical production environments** running in parallel. One environment (call it "blue") is currently live and serving all traffic. The new version is deployed and fully prepared on the other environment ("green") while blue continues handling users. Once green is validated, a router or load balancer flips all traffic from blue to green in a single, near-instantaneous switch. Blue stays running but idle; no longer receiving traffic, but available as an instant rollback target.

The key innovation here is that **nothing happens to the live environment during the deployment**. Green is built and tested while blue runs normally, and the transition itself is a traffic switch rather than a deployment operation. If something goes wrong after the switch, rollback is almost instant: flip the traffic back to blue, which was never changed.

### Strategy Comparison

Rolling and canary strategies both involve running two versions of the application **simultaneously under production load** for some period of time, creating a mixed-version state. Blue/green avoids mixed-version states entirely: all traffic is always going to one environment, and the other is either being prepared or standing by as a rollback option. The tradeoff is cost: we're running two full environments and paying for all of those infrastructure resources, even when one is idle.

This also changes the nature of rollback procedures. In a rolling or canary release, reversing a bad deploy means re-deploying the old version (which takes time). In blue/green, rollback is a traffic switch back to blue, which typically takes seconds. The blue environment was never modified during the deployment, so it's immediately available.

### Benefits

Blue/green is particularly well-suited for changes where version coexistence is genuinely problematic. For example, a database schema migration where the old and new application code can't safely share the same data layer at the same time.

- **Near-instant rollback.** The blue environment is left ready for traffic. If the green deployment has a critical problem, you flip traffic back to the blue environment, and you're back to the previous state within seconds. This is the fastest and cleanest rollback option of the four strategies we cover. For high-stakes releases where the ability to reverse course instantly is worth the infrastructure cost, blue/green is the strongest option.
- **Zero mixed-version state.** There's no window where some users are on the old version and some are on the new. The transition is binary: before the switch, everyone is on blue; afterwards, everyone is on green. This eliminates the coexistence concerns that other deployments introduce.
- **Validation before cutover.** Green can be thoroughly tested in a production-identical environment, including realistic load testing, integration checks, and smoke tests, before any user traffic touches it.

### Tradeoffs

| Metric | Notes |
|---|---|
| **Speed** | The actual traffic cutover is the fastest of any strategy, we switch over all at once to the green environment. But preparing a full second environment takes upfront time and cost. |
| **Risk** | Low per-deployment risk due to instant rollback. However, the traffic switch is still "all at once". Every user moves to the new version at the same moment. Large, subtle bugs that only emerge at scale may not be caught before full rollout. |
| **Complexity** | High infrastructure cost. Two full production environments must be maintained, provisioned, and kept in sync. This is more tractable with IaC and cloud infrastructure than it was with on-premise servers. |
| **Observability needs** | Moderate. You need enough monitoring to quickly detect problems after the cutover, since all users transition simultaneously and there's no gradual exposure. |

### Gradual rollouts

Describe each type listed as subheadings below 
- Describe the deployment style
- clarify how it's different from previously describe deployment types
- Describe key benefits and tradeoffs. Speak to metrics like: speed, risk, complexity, observability needs

## Decoupling Shipment from Feature Release: Config vs code

Define configuration vs code and configuration management with examples
what are the problems it solves
what are the tradeoffs around complexity and management
how does it help us release features without shipping a new version

## Summary



## Check for Understanding

CFU Ideas:
Which deployment strategy is designed to reduce risk by sending a small percentage of traffic to the new version first?
Blue/green deployment is best described as...?
What’s the primary idea behind “blast radius” in deployments?