# Automation Beyond App Deploys: Infrastructure-as-Code

## Learning Goals

- Define the concept of Infrastructure-as-Code
- Describe benefits and common use cases for Infrastructure-as-Code

## Vocabulary and Synonyms

| Vocab | Definition | Synonyms | How to Use in a Sentence |
| --------- | --------- | -------- | --------- |
| Infrastructure as Code (IaC) | Defining servers, databases, networks, and permissions in code instead of manually creating and configuring each piece of infrastructure. | IaC, infrastructure automation, config as code | "By adopting infrastructure as code, our team was able to spin up an identical staging environment in minutes by running the same template we use for production." |
| Declarative Automation | Describing the final state you want and letting the system figure out how to achieve it. For example: "There should be 3 servers running," rather than writing step-by-step instructions to create them. | Declarative configuration, desired-state configuration | "We wrote a declarative automation template that specifies our application needs four running instances, and the platform handles adding or removing servers to match that target." |
| Desired State Systems | Systems that constantly check whether reality matches the defined configuration. If something drifts from the expected setup, the system detects and corrects it automatically. | Self-healing systems, drift correction, configuration enforcement | "When someone manually changed a server's settings outside of our normal process, our desired state system detected the drift and automatically reverted the configuration to match what was defined in code." |
| Configuration as Code (CaC) | Defining application and system configuration in version-controlled files rather than setting it manually through dashboards or ad hoc scripts. | CaC, config as code | "After we moved to configuration as code, every change to our pipeline went through a pull request, so we had a clear record of what changed and when." |

## What is Infrastructure-as-Code?

We've established that a release pipeline moves code through a sequence of validated steps from a developer's commit all the way to production. But the environments those steps run in (staging, production, and any infrastructure in between) also have to be built and maintained somehow. How do we make sure that a staging environment reliably mirrors production? How do we recover quickly if a server fails or a new region needs to be brought online? And how do we know, six months from now, exactly what the infrastructure looked like when a specific incident occurred?

**Infrastructure-as-Code** (IaC) is the practice of defining and managing infrastructure, like servers, databases, network settings, permissions, and more, through code and configuration files rather than by clicking through a dashboard or running commands by hand. Instead of logging into a console and manually provisioning a database instance, an engineer writes a file that describes what the database infrastructure should look like. That file is then read by a tool that creates or updates the real infrastructure to match.

It's worth noting that defining a CI/CD pipeline as code is closely related to IAC, but is not typically considered infrastructure-as-code. CI/CD configuration files define the process to to build, test, and deploy software. They specify the logic of the automation itself, not the hardware or cloud resources being managed. This kind of definition of process and configuration is typically called configuration-as-code. Infrastructure-as-Code refers specifically to defining and provisioning IT infrastructure, such as servers, networks, and databases, using machine-readable files.

A core idea of IAC is desired state: rather than writing step-by-step instructions for how to build something, we declare what the end result should be and let the tool figure out what resources need to be added, removed, or updated to match. 
- For example, if a database with a certain configuration should exist, we say so in the file. If it already exists and matches the description, nothing changes. If it doesn't exist yet, the tool creates it. 
 
This approach is sometimes called declarative configuration, and it's what separates IaC from simply automating a series of setup commands. 

These infrastructure configuration files are typically plain text (often structured as .yaml files), so they can live in version control alongside application code. When infrastructure is documented as code, changes to an application's infrastructure go through the same review process as code changes: they're proposed, reviewed, discussed, and merged. Teams can see the full history of every infrastructure change, who made it, and why.

## Use Case: Defining Environments 

One of the most common uses of IaC is defining the infrastructure for the environments in a release pipeline: development, staging, and production. We've talked about how environment parity matters: if staging and production differ in meaningful ways, bugs that only appear in one of them will slip through the pipeline undetected.

IaC makes parity easier to achieve and easier to maintain. If both staging and production are defined using the same base configuration file (environment-specific values like instance sizes or replica counts can be supplied separately), then structural drift between them becomes much harder to introduce accidentally. Adding a new service to the staging environment means adding it to the configuration file; if that change isn't also applied to production's configuration, the mismatch is explicit and visible rather than hidden.

This approach also makes onboarding new environments straightforward. If a team needs a second staging environment for a large feature branch, they apply the same configuration file with a different set of environment-specific values. No manual setup, no risk that the new environment is missing something the original staging environment had.

## Benefits of IAC

We talked a bit in the CI/CD lesson around benefits to our release pipeline that we get when we automate and use code to define our pipeline steps: reducing drift, having consistent set up and configuration of resources, and being able to see the history of the pipeline in version control. IAC gives us those same kinds of benefits for our underlying infrastructure and environments.

### Reducing Configuration Drift

In any environment managed by hand, drift is almost inevitable. One team member applies a security patch to the production server but forgets to apply it to staging. Another adds a new environment variable to fix a bug but doesn't document the change. Over time, environments that are supposed to be identical quietly diverge in ways that are hard to detect and even harder to debug. When a bug only appears in production and not in staging, configuration drift is often the culprit.

IaC addresses this directly. Because the environment is defined in a file, the file is the authoritative description of what should exist. Running the same configuration file against two different environments will create matching resources and infrastructure. If someone makes a manual change to an environment outside of the IaC process, that change is not reflected in the file and can be detected or even automatically corrected the next time the configuration is applied.

### Repeatability and Auditability

IaC makes environments repeatable in the same way that CI/CD makes release pipelines repeatable. Spinning up a new staging environment doesn't require following a long setup document that may be out of date — it means running the same configuration file used to build the previous one. This is especially valuable in disaster recovery scenarios, where quickly rebuilding a failed environment can be the difference between a minor incident and an extended outage.

Auditability comes from version control. Every change to infrastructure is a change to a file, and every change to a file is a commit with an author, a timestamp, and a message. Teams can answer questions like "What did the production database configuration look like before last week's deploy?" or "When did we add that new environment variable?" without digging through logs or asking around.

## Summary

When infrastructure is managed by hand, that process is nearly impossible to reproduce exactly. Memory fades, steps get skipped, and the people who know the "right" way to set something up become single points of failure. Infrastructure-as-Code solves this by moving those decisions into files: machine-readable declarations that describe what cloud resources should exist and how they should be configured. Because those declarations live in plain text files in a repository, every infrastructure change goes through the same pull request and review workflow as the rest of your codebase. Tools that support IaC read those files and reconcile the real world with what's described, creating or adjusting resources as needed. The practical effect is that standing up a new environment looks less like a project and more like running a command.

This approach directly addresses two problems that tend to compound over time in production systems. 
- The first is configuration drift — the gradual, often invisible divergence between environments that causes bugs to appear in production but not in staging. Because IaC files are the authoritative record of what an environment should look like, untracked manual changes stand out rather than quietly accumulating. 
- The second problem is institutional knowledge loss. Runbooks and setup guides go stale; a well-maintained IaC repository doesn't, because it has to stay in sync with the actual infrastructure or things break. New team members can understand the full shape of the infrastructure by reading the same files that built it, and disaster recovery becomes a repeatable process rather than a high-stakes improvisation.

## Check for Understanding

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 4d644806-5086-4716-8723-410aa6e4c18c
* title: Infrastructure-as-Code
##### !question

A startup is growing quickly and needs to launch identical cloud environments for three new regional markets. Instead of having an engineer manually click through the cloud provider's console to recreate each environment from memory, the team applies the same configuration file to each region.

Which statement best describes the core idea that makes this approach work?

##### !end-question
##### !options

* Infrastructure is provisioned by writing step-by-step scripts that run setup commands in the correct order
* Infrastructure is defined by declaring a desired end state, and a tool reconciles real resources to match that description
* Infrastructure is cloned by copying a running environment's settings into a new region automatically
* Infrastructure is managed by the cloud provider, so engineers only need to specify a budget and a region

##### !end-options
##### !answer

* Infrastructure is defined by declaring a desired end state, and a tool reconciles real resources to match that description

##### !end-answer
##### !explanation

The core idea of Infrastructure-as-Code is declarative configuration: you describe what you want the infrastructure to look like, and the IaC tool determines what needs to be created, updated, or left alone to reach that state. This is different from writing imperative step-by-step scripts. The other options either describe manual processes, mischaracterize how IaC tools work, or confuse IaC with fully managed services.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 2817b244-00ea-4ace-8597-974d57943ffa
* title: Infrastructure-as-Code
##### !question

Over the course of a year, several engineers at a company made small "quick fix" changes to the production environment directly through the cloud console without updating any shared documentation. When a new engineer joins and spins up a fresh environment from the team's setup guide, it behaves differently from production in ways nobody can fully explain.

Which problem does Infrastructure-as-Code most directly address in this situation?

##### !end-question
##### !options

* Slow onboarding caused by a lack of written documentation about the codebase
* Configuration drift caused by untracked manual changes that leave environments out of sync
* High cloud costs caused by redundant resources accumulating across environments
* Deployment failures caused by untested code reaching production without CI checks

##### !end-options
##### !answer

* Configuration drift caused by untracked manual changes that leave environments out of sync

##### !end-answer
##### !explanation

Configuration drift is the gradual divergence between environments caused by changes made outside of a controlled, tracked process. IaC combats this by making the configuration file the authoritative description of what an environment should look like. The other options describe real engineering problems, but they aren't what IaC is designed to solve.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 955d9fe5-f533-4947-963d-669abbec17be
* title: Infrastructure-as-Code
##### !question

A company's data center in one region goes offline unexpectedly. The team needs to restore service in a different region as quickly as possible. One engineer says, "Good thing we've been managing our infrastructure with IaC, we can have this back up fast." A colleague who is newer to the team asks why that matters for recovery speed.

What is the best explanation?

##### !end-question
##### !options

* IaC tools automatically detect outages and begin provisioning replacement infrastructure without human input
* The infrastructure is already fully defined in a file that can be applied to the new region, so recovery doesn't depend on anyone remembering how things were set up
* IaC keeps a live replica of the environment running in a standby region, ready to take over immediately
* Version control stores backups of the application's data, so restoring infrastructure also restores any data that was lost

##### !end-options
##### !answer

* The infrastructure is already fully defined in a file that can be applied to the new region, so recovery doesn't depend on anyone remembering how things were set up

##### !end-answer
##### !explanation

One of IaC's most practical benefits is repeatability: because the environment is fully described in a file, rebuilding it elsewhere is a matter of applying that file rather than reconstructing the setup from memory, outdated runbooks, or one developer's knowledge. 

IaC does not automatically detect outages, maintain live standby replicas, or back up application data. Those are separate concerns addressed by other tools and architectural patterns.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 7d1a3bce-30bf-42e1-8e12-0b74bf0fa8e0
* title: Infrastructure-as-Code
##### !question

A team uses IaC to manage all of their cloud infrastructure. During a late-night incident, an engineer adds a new environment variable directly through the cloud console to resolve an issue quickly, intending to "update the config file tomorrow." That update never happens.

What is the most significant risk created by this situation?

##### !end-question
##### !options

* The cloud provider may automatically revert the console change since it conflicts with the team's billing plan
* The IaC configuration file no longer reflects the actual state of the environment, undermining it as a reliable source of truth
* The environment variable will be lost the next time anyone logs into the console
* The incident response was handled incorrectly because IaC tools should have been used to detect the root cause

##### !end-options
##### !answer

* The IaC configuration file no longer reflects the actual state of the environment, undermining it as a reliable source of truth

##### !end-answer
##### !explanation

When changes are made outside the IaC process, the configuration file drifts from the real state of the environment. This means the file can no longer be trusted as an accurate description of what's actually running — the exact problem IaC is meant to prevent! Future runs of the IaC tool might overwrite the manual change, and the team loses the audit trail and review process that version-controlled infrastructure provides. Cloud providers do not automatically revert console changes to match IaC files.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->