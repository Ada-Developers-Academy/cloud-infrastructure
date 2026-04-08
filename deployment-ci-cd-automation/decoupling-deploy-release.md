# Decoupling Shipping Code and Releasing Features

## Learning Goals

- Describe configuration vs code and reasons for documenting configuration like code
- Define feature flags and common use cases for them

## Vocabulary and Synonyms

| Vocab | Definition | Synonyms | How to Use in a Sentence |
| --------- | --------- | -------- | --------- |
| Feature Flagging | A way to turn features on or off without redeploying code. Useful for gradual rollouts, testing features with small groups, or quickly disabling something if it breaks in production. | Feature flags, feature toggles, feature switches | "We shipped the new dashboard behind a feature flag so we could enable it for internal testers first before rolling it out to all users." |
| Configuration Management | Managing application settings separately from code. Things like API keys, feature flags, and environment variables are stored securely and injected at runtime rather than hardcoded into the application. | Runtime configuration, config management, secrets management | "We use configuration management to store our database credentials and feature flags outside of the codebase, so we can update them without redeploying the application." |

## Configuration vs. Code

The deployment strategies we've looked at so far all share one assumption: releasing a feature to users means deploying a new version of the software. But what if that assumption is wrong, or at least optional? Many teams have found that the moment a new feature "goes live" doesn't have to be tied to a deployment at all. To understand how, we need to pull apart two things that often get lumped together: **code** and **configuration**.

Think about the last time we deployed an app to a platform like Render or Heroku. We probably set some environment variables alongside the code: a database URL, an API key, maybe a port number. Those variables influenced how the app behaved without being part of the application code itself. That's the main idea behind configuration.

**Code** is application logic: the functions, routes, and business rules that define what the software does. Changing behavior that lives in code requires going through a build and deploy cycle.

**Configuration** is the collection of values and settings that shape how the code behaves at runtime, without the code itself changing. Common examples include:
- The URL or API key for a third-party service
- A timeout duration for database connections  
- A dollar threshold that changes free shipping eligibility
- A flag that controls whether a new feature is visible to users

A critical characteristic of configuration is that it can be updated without redeploying the application. The app reads those values when it starts (or sometimes continuously at runtime), and a change to the config file or environment variable immediately or very quickly changes the behavior users see. This separation is useful for routine operational values, but it becomes especially powerful when applied to feature releases.

## Feature Flags

Let's look at a scenario: a team has spent three weeks building a redesigned checkout flow. The new code is finished, tested, and ready. But leadership wants to do a soft launch: show it to 10% of users first and watch conversion rates before rolling it out to everyone.

Without feature flags (also called feature toggles or feature switches), this is complicated. We'd likely use a deployment strategy like a canary release, routing a slice of traffic to a new version of the application. That works, but it involves infrastructure changes and means running two different deployed versions of the app simultaneously.

With a **feature flag**, the approach is different. The new checkout flow ships inside the existing codebase, wrapped in a conditional check that might look something like:

```python
if feature_flags.get("new_checkout_enabled"):
    show_new_checkout_flow()
else:
    show_existing_checkout_flow()
```

The flag `new_checkout_enabled` defaults to `false`. The code deploys as part of a normal release. Users see nothing new. When the team is ready, they flip the flag to `true`, no deployment needed. A configuration value changed, and the feature is live.

More precisely, in this scenario we'd flip the flag to `true` for 10% of users. When metrics look good, we ramp to 25%, then 50%, then 100%. If something unexpected appears at any point, we flip the flag back off. Again, no deployment, no rollback procedure, just a configuration change that takes effect immediately.

Feature flags can be applied in a few distinct ways:

- **Kill switches**: A flag whose only job is to turn a problematic feature off in an emergency. If a new payment integration starts throwing errors, flipping the kill switch instantly routes users back to the old flow while engineers investigate.

- **User-targeted flags**: The feature is visible only to specific users or groups, like internal team members, beta testers, or users in a particular region. This is one of the most common ways to validate a feature under real production conditions without exposing it to everyone.

- **Percentage rollouts**: The flag is enabled for a defined percentage of traffic. Combined with monitoring, this is one of the safest ways to release a high-risk feature gradually.

## Configuration Management

As soon as a team starts using feature flags seriously, they run into a new problem: managing them. A handful of environment variables in a `.env` file is easy. Dozens of flags across multiple environments, with different values per user segment, modified by multiple team members, is not.

**Configuration management** is the practice (and the tooling) for storing, versioning, validating, and safely rolling out configuration values across environments. Where a basic setup might be environment variables set by hand on a server, a proper configuration management system adds:

- **A central store** so all configuration lives in one place rather than spread across server settings, config files, and someone's notes
- **Access controls** so only authorized people can change which flags are enabled in production
- **Validation** so a typo in a service URL or an impossible percentage value is caught before it goes live
- **Audit logs** so there's a record of every change — what changed, who changed it, and when
- **Rollback** so if a configuration change causes problems, reverting it is as fast and safe as reverting code

This probably sounds familiar. Configuration management is essentially applying the same rigor to config values that CI/CD applies to code changes. And for good reason: a configuration change that accidentally enables a half-finished feature for all users is a production incident, even if no code was deployed. The more powerful configuration becomes as a release mechanism, the more it needs to be treated with the same care as code.

## Tradeoffs

Feature flags and configuration management solve real problems, but they add their own complexity:

- **Flags accumulate.** A flag that launched a feature six months ago and has been at 100% ever since should be removed. The conditional logic supporting it is dead code. But cleanup is easy to defer, and teams that don't build a flag retirement process end up with codebases full of conditions that no one's sure are still meaningful.

- **Combinations multiply.** If our application has ten flags, each of which can be on or off, that's technically 1,024 possible states. In practice most combinations don't matter, but testing "flag A on, flag B off" versus "both on" versus "both off" adds real surface area to our QA process. This gets harder to manage as flags multiply.

- **Configuration is still a change.** It can feel lower-stakes to flip a flag than to deploy code. But a configuration change that enables a broken feature for 100% of users is an incident. The feeling of safety can cause teams to skip the review and validation steps they'd apply to a code change — which is exactly when configuration mistakes happen.

- **Tooling investment requires resources.** Basic feature flags in environment variables are cheap to set up. Configuration management systems with staged rollouts, user targeting, audit logs, and validation take time to build or cost money to buy. For a small team or early-stage product, that overhead may not be worth it yet.

None of these tradeoffs make feature flags a bad idea; for teams doing active product development with continuous delivery, they're often essential! But they're most effective when the team treats them as a system to be managed, not just a trick to skip deployments.

## The Bigger Picture

Here's the shift in thinking these patterns enable: **shipping code and releasing features are two separate decisions**, and they don't have to happen at the same time.

Shipping code is a technical operation — build, test, deploy. It happens on a pipeline schedule, following the safety practices we've covered. Releasing a feature is a product decision: when is this ready for users, how broadly, and with what guardrails? Feature flags and configuration management let those decisions happen independently, made by different people on different timelines.

A team operating this way might follow a sequence like:
1. Ship code containing a new feature, entirely behind a flag that defaults to off
2. Enable the flag for internal users to validate behavior in the real production environment
3. Gradually ramp the flag to 10%, 25%, 50% of external users while watching error rates and key metrics
4. Complete the rollout — or flip the flag off instantly if something goes wrong at any stage

The deployment happened at step 1 and didn't change. Everything from step 2 onward is configuration, that's the decoupling in action!

## Summary

Software teams often treat deploying code and releasing features as a single event, but they don't have to be. **Code** is the logic baked into the application itself; changing it means going through a build and deploy cycle. **Configuration** is a layer of values (timeouts, thresholds, flags) that the running application reads at runtime and that can be changed without touching a deployment. 

Feature flags live in that configuration layer: a conditional in the codebase routes users to either the old or new behavior depending on the flag's current value. This means a team can ship a half-finished or experimental feature on a regular deploy cadence, keep it invisible to users while it's set to off, then enable it for a small internal group, or a percentage of real users, whenever they're ready - all without triggering a new deployment!

When using feature flags, it's important to build flag retirement into our plan as a discipline. Flags that reached 100% rollout months ago and never got cleaned up accumulate as dead conditional logic. Skipping the clean-up step trades a short-term convenience for long-term codebase complexity: those conditionals left behind have to be decoded by future engineers before they can safely make changes. Teams that treat feature flags as a temporary tool with a planned end date get the benefits: gradual exposure, instant kill switches, audience-targeted launches, without the long-term overhead.

The power of this pattern grows with discipline. A flag flipped carelessly in production is still a production incident, so configuration needs the same guardrails applied to code: a centralized store, access controls, validation, and an audit trail of who changed what and when. That's what configuration management systems provide. Configuration management systems bring version control, validation, and rollback to this layer the same way CI/CD does for source code.

## Check for Understanding