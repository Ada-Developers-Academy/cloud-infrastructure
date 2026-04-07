# Deployment Strategies & Risk Management

## Learning Goals

Core question: How do we reduce blast radius when shipping?

## Vocabulary and Synonyms

| Vocab | Definition | Synonyms | How to Use in a Sentence |
| --------- | --------- | -------- | --------- |
| Change Management | The process of introducing updates to a system safely. It includes planning, testing, reviewing the potential impact of a change, and having a rollback plan ready in case something goes wrong. | Release management, change control | "Before deploying the database migration, the team followed their change management process by documenting the steps, reviewing the rollback plan, and scheduling the update during a low-traffic window." |
| Feature Flagging | A way to turn features on or off without redeploying code. Useful for gradual rollouts, testing features with small groups, or quickly disabling something if it breaks in production. | Feature flags, feature toggles, feature switches | "We shipped the new dashboard behind a feature flag so we could enable it for internal testers first before rolling it out to all users." |
| Configuration Management | Managing application settings separately from code. Things like API keys, feature flags, and environment variables are stored securely and injected at runtime rather than hardcoded into the application. | Runtime configuration, config management, secrets management | "We use configuration management to store our database credentials and feature flags outside of the codebase, so we can update them without redeploying the application." |

## Deployment Types

Why have deployment strategies - include some historical context and issues with all at once/big bang deploys

Describe each type listed as subheadings - clarify how it's different from previously describe deployment types

Describe key benefits and tradeoffs - metrics like: speed, risk, complexity, observability needs

### Rolling deployments
### Canary releases
### Blue/green
### Gradual rollouts


## Change After Shipping: Config vs code

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