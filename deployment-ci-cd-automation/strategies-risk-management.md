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

### Benefits

This rolling strategy helps reduce risk compared to an all-at-once deploy. As the new version is rolled out, we can monitor the updated servers for issues. If a problem emerges, the rollout can be stopped at any point before the new version reaches the full fleet.

- **Reduced customer impact**: At any point during the rollout, only a fraction of users are running the new version. A bug affects that fraction, not everyone.
- **No downtime**: Because instances are replaced one at a time while others continue serving traffic, the application stays available throughout the deployment.
- **Simple to implement**: Most managed container platforms and cloud orchestration tools support rolling deployments natively. It's often the default strategy for teams that haven't made a more deliberate choice.

### Tradeoffs

| Metric | Notes |
|---|---|
| **Speed** | Slower than big bang; the rollout takes time proportional to instance count. Configurable, but you're trading speed for safety. |
| **Risk** | Lower than big bang, but not zero. If a bug only surfaces under specific load patterns, it might not appear until the rollout is nearly complete. |
| **Complexity** | Low. It's a sequential replacement of instances, and most orchestration tools manage this automatically. |
| **Observability needs** | Moderate. We need monitoring in place to notice if the new version is behaving differently from the old one while both are running. Version-tagged metrics help here. |

During a rolling deployment, our application is briefly running two versions at once. For many changes this is fine, but for changes that alter database schemas, API contracts, or message formats, both versions need to be able to coexist. If the new version writes data in a format the old version can't read, we'll have a problem mid-rollout.

### Canary releases
### Blue/green
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