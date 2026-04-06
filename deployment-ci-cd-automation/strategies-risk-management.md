# Deployment Strategies & Risk Management

## Learning Goals

Core question: How do we reduce blast radius when shipping?

## Vocabulary and Synonyms

| Vocab | Definition | Synonyms | How to Use in a Sentence |
| --------- | --------- | -------- | --------- |
| Configuration Management | Managing application settings separately from code. Things like API keys, feature flags, and environment variables are stored securely and injected at runtime rather than hardcoded into the application. | Runtime configuration, config management, secrets management | "We use configuration management to store our database credentials and feature flags outside of the codebase, so we can update them without redeploying the application." |

## Deployment Types

Why have deployment strategies 
- include some historical context and issues with all at once/big bang deploys

Describe each type listed as subheadings 
- clarify how it's different from previously describe deployment types

Describe key benefits and tradeoffs 
- metrics like: speed, risk, complexity, observability needs

### Rolling deployments
### Canary releases
### Blue/green
### Gradual rollouts


## Change After Shipping: Config vs code

what is configuration vs code
what are the problems it solves
what are the tradeoffs around complexity and management
how does it help us release features without shipping a new version

## Summary



## Check for Understanding

CFU Ideas:
Which deployment strategy is designed to reduce risk by sending a small percentage of traffic to the new version first?
Blue/green deployment is best described as...?
What’s the primary idea behind “blast radius” in deployments?