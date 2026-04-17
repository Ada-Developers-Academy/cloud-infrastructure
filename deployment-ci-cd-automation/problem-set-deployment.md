# Problem Set: Deployment, CI/CD, & Automation

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 9b525614-0e9a-415e-9a25-2001f6b8a88b
* title: Problem Set: Deployment, CI/CD, & Automation
##### !question

A team's application passes all tests in the development environment but crashes immediately after being deployed to production. After investigation, the team discovers that production is running a different version of a key dependency than the development environment. Which concept does this failure illustrate?

##### !end-question
##### !options

* The need for more automated unit tests in the build step
* A lack of environment parity between development and production
* An incorrectly configured release pipeline trigger
* The risk of deploying artifacts that have not been versioned

##### !end-options
##### !answer

* A lack of environment parity between development and production

##### !end-answer
##### !explanation

Environment parity is the practice of keeping development, staging, and production environments as structurally similar as possible, including runtime versions and dependency configurations. When parity breaks down, bugs that only surface under production conditions slip through undetected. This is a classic example of the "works on my machine" problem that environment parity is designed to prevent.

##### !end-explanation
### !end-challenge
<!--prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 1c1e9e62-893d-4042-8cd7-4b6f90d6e317
* title: Problem Set: Deployment, CI/CD, & Automation
##### !question

A developer pushes code that causes a unit test to fail during the build step of the release pipeline. What should happen next, according to standard release pipeline behavior?

##### !end-question
##### !options

* The pipeline skips the failing test and continues deploying to staging so the team can investigate in a real environment
* The pipeline stops at the failing step and alerts the team that there is an error requiring investigation before the change can advance
* The pipeline automatically promotes the last successful artifact to staging while the team fixes the failing test
* The pipeline redeploys the current production artifact to staging as a precaution

##### !end-options
##### !answer

* The pipeline stops at the failing step and alerts the team that there is an error requiring investigation before the change can advance

##### !end-answer
##### !explanation

At each step of a release pipeline there are quality checks. When a check fails, the pipeline stops at that step and alerts the team rather than letting a broken change advance to the next stage. This prevents untested or broken code from ever reaching staging or production. The pipeline does not skip tests, auto-promote previous artifacts, or take protective action on staging in response to a build failure.

##### !end-explanation
### !end-challenge
<!--prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 853b1bfb-f3cd-498e-82cd-a19b15e02fd8
* title: Problem Set: Deployment, CI/CD, & Automation
##### !question

A team is preparing to release a new user authentication flow. They test it in their development environment and everything looks good. Why should they deploy to a staging environment before releasing to production?

##### !end-question
##### !options

* Staging environments are required by most cloud providers before a production deployment can be triggered
* Staging environments allow the team to validate changes under realistic conditions, catching issues that may only surface at production scale or with real data volumes
* Deploying to staging creates a backup of the current production environment in case something goes wrong
* Staging environments run a stricter set of unit tests than the build step, which are not available earlier in the pipeline

##### !end-options
##### !answer

* Staging environments allow the team to validate changes under realistic conditions, catching issues that may only surface at production scale or with real data volumes

##### !end-answer
##### !explanation

Staging environments closely mirror production so that changes can be validated under realistic conditions before reaching real users. The lesson example illustrates this directly: a bug in the password reset logic only surfaced in staging when tested against realistic data volumes, and was caught before it could impact users in production. The other options misrepresent what staging environments are and how they work.

##### !end-explanation
### !end-challenge
<!--prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 4Bq2mRwX7nKpLsJ9cTzYeA3dFhV0gU
* title: Problem Set: Deployment, CI/CD, & Automation
##### !question

A pull request is merged. Which sequence best represents a typical, safe path to production?

##### !end-question
##### !options

* Merge → deploy to production → run tests → monitor
* Merge → build artifact → run automated tests → deploy to staging → validate → deploy to production → monitor
* Merge → deploy to staging → build artifact → deploy to production → run tests
* Merge → run automated tests → deploy to production → build artifact → monitor

##### !end-options
##### !answer

* Merge → build artifact → run automated tests → deploy to staging → validate → deploy to production → monitor

##### !end-answer
##### !explanation

After a PR is merged:
1. The release lifecycle begins with a **build** step that turns source code into a versioned, deployable artifact. 
2. Automated tests run against that artifact before it goes anywhere. 
3. The artifact is then promoted to a **staging environment**, which mirrors production as closely as possible, so that integration issues and configuration mismatches can be caught before real users are affected. 
4. Only after staging validation passes does the artifact get deployed to **production**, followed by verification and ongoing monitoring. 

Building artifacts after deployment (options C and D) or skipping staging entirely (option A) removes critical safety gates that prevent broken changes from reaching users.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 9wZnPk3RsTqBmYcXeL7gJvD2hA0fUi
* title: Problem Set: Deployment, CI/CD, & Automation
##### !question

Which option best distinguishes Continuous Integration (CI), Continuous Delivery, and Continuous Deployment?

##### !end-question
##### !options

* CI deploys to production automatically; Continuous Delivery requires human approval for staging; Continuous Deployment skips testing entirely
* CI, Continuous Delivery, and Continuous Deployment are different names for the same automated pipeline, the only differences are in the tools used
* CI automates building and testing; Continuous Delivery automates through staging tests and keeps code ready for release; Continuous Deployment deploys to production automatically once all checks pass
* CI automates deployment to production environments; Continuous Delivery adds a staging environment; Continuous Deployment adds automated tests

##### !end-options
##### !answer

* CI automates building and testing; Continuous Delivery automates through staging tests and keeps code ready for release; Continuous Deployment deploys to production automatically once all checks pass

##### !end-answer
##### !explanation

These three practices build on each other and each extends automation one step further down the pipeline. 
- **Continuous Integration (CI)** covers the build and test stages. Every pull request and merge triggers an automated sequence that compiles code, runs tests, and produces an artifact (or raises an alert if something breaks). 
- **Continuous Delivery** picks up from a passing CI run and automates deployment through staging and any additional validation, leaving the code in a state that *could* go to production at any time, but a human still makes the final call. 
- **Continuous Deployment** removes that human approval step entirely: if all automated checks pass, the pipeline deploys to production with no manual intervention. 

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 79ef206f-6c54-457f-98cc-42c261594c7a
* title: Problem Set: Deployment, CI/CD, & Automation
##### !question

A healthcare software company wants to adopt Continuous Deployment to increase their release velocity. Which of the following factors should give them the most pause before doing so?

##### !end-question
##### !options

* Their engineering team is large, which makes coordinating manual approvals slow
* Regulatory requirements mandate human review before any change reaches patients, and some deployments include database schema migrations that cannot easily be undone
* Their current test suite takes 20 minutes to run, which would slow down the automatic deploy trigger
* They release multiple times per week, which would cause the automated pipeline to run too frequently

##### !end-options
##### !answer

* Regulatory requirements mandate human review before any change reaches patients, and some deployments include database schema migrations that cannot easily be undone

##### !end-answer
##### !explanation

Continuous Deployment is not the right fit when compliance rules require human sign-off before changes go live, or when deployments involve changes with lasting side effects that are hard to reverse — such as schema migrations. In healthcare, both concerns often apply simultaneously. Continuous Delivery, which keeps the human approval step, is generally the more appropriate model in these contexts.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: ecc5b399-e2b2-43b4-a130-38365f161b79
* title: Problem Set: Deployment, CI/CD, & Automation
##### !question

After an unexpected production incident, a team's engineering manager asks: "Exactly what changed in our infrastructure last Tuesday, and who approved it?" Without Infrastructure-as-Code, the team struggles to answer — the changes were made manually and no one kept detailed notes.

Why does storing infrastructure definitions in version control make this kind of question easy to answer?

##### !end-question
##### !options

* Version control systems send automatic alerts whenever infrastructure resources are modified
* Every change to a configuration file creates a commit record with an author, timestamp, and description, forming a complete audit trail
* Version control tools archive cloud console activity logs so changes are always traceable
* Cloud providers require infrastructure files to be version-controlled before allowing changes to production

##### !end-options
##### !answer

* Every change to a configuration file creates a commit record with an author, timestamp, and description, forming a complete audit trail

##### !end-answer
##### !explanation

When infrastructure is defined in files stored in version control, every modification goes through a commit — capturing who made the change, when, and ideally why. This creates a reliable audit trail that teams can review at any time. Version control does not monitor the cloud console directly or archive cloud provider activity logs; it only tracks changes made to the files themselves.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 0dc610cc-1443-4975-ac28-d21da9d9bf8d
* title: Problem Set: Deployment, CI/CD, & Automation
##### !question

A team's repository contains two types of files: one set that specifies the sequence of build, test, and deploy steps their pipeline executes on every pull request, and another set that specifies the virtual machines, databases, and network settings their application runs on.

How should these two types of files be categorized?

##### !end-question
##### !options

* Both are Infrastructure-as-Code because they are both written in code and checked into version control
* The pipeline step definitions are configuration-as-code; the resource definitions are Infrastructure-as-Code
* The resource definitions are configuration-as-code; the pipeline step definitions are Infrastructure-as-Code
* Both are artifact management files because they define the outputs needed for deployment

##### !end-options
##### !answer

* The pipeline step definitions are configuration-as-code; the resource definitions are Infrastructure-as-Code

##### !end-answer
##### !explanation

Infrastructure-as-Code specifically refers to defining and provisioning cloud resources (servers, databases, networks, and similar infrastructure) in machine-readable files. 

Defining the steps of a CI/CD pipeline in code is a closely related practice, but it describes automation logic rather than cloud resources, and is more accurately called configuration-as-code. 

Both practices share the benefits of version control and repeatability, but they apply to different layers of the system.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 7d1a3bce-30bf-42e1-8e12-0b74bf0fa8e0
* title: Problem Set: Deployment, CI/CD, & Automation
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

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 5rEqMnW1kBsLzCpYvJxTgH8dF3aU0o
* title: Problem Set: Deployment, CI/CD, & Automation
##### !question

A production deployment is causing errors. Which situation is most likely to make a rollback insufficient or risky?

##### !end-question
##### !options

* The deployment only updated application code and no other system components were changed
* The deployment included a database migration that removed a column from a table 
* The previous artifact version is stored in the artifact repository and can be retrieved quickly
* The team has monitoring gates configured to detect elevated error rates automatically

##### !end-options
##### !answer

* The deployment included a database migration that removed a column from a table 

##### !end-answer
##### !explanation

Rollback is straightforward when a deployment only changes application code: re-deploy the previous artifact, and the system returns to its prior state. Complications arise when a deployment also changes **persistent state**, in this case, the database schema. 

If a migration dropped a column that the previous version of the application queries, rolling back the code leaves the old application running against a schema it was never designed for. Every query that referenced the dropped column will now fail, potentially making the situation worse than the original error. 

The other options (A, C, D) all describe conditions that make rollback *easier* and more reliable, not harder.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: gT7fV2yMoE9iQcUkBs4lDnXwAp
* title: Problem Set: Deployment, CI/CD, & Automation
##### !question

After a bad deployment, a team determines that reverting to the previous version would be unsafe because the release included a migration that restructured how customer records are stored. They choose to write a targeted patch and ship it as a new release instead. This approach is best described as a:

##### !end-question
##### !options

* Rollback
* Monitoring gate
* Roll-forward
* Expand-contract migration

##### !end-options
##### !answer

* Roll-forward

##### !end-answer
##### !explanation

A roll-forward means responding to a broken deployment by developing and releasing a corrective fix rather than reverting to the prior version. When a deployment has changed persistent state — like restructuring how records are stored — rolling back the code may leave the old application incompatible with the new database structure. In those cases, pushing a fix forward is often the safer and faster path to recovery.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: wC2pS9eKmNzYbX5tGvIqOjUdFr
* title: Problem Set: Deployment, CI/CD, & Automation
##### !question

A team finishes a rollback and assumes the incident is closed. Fifteen minutes later, they learn users are still experiencing slow page loads. What step did the team most likely skip?

##### !end-question
##### !options

* They did not write a post-mortem before closing the incident
* They did not verify that monitoring signals returned to their pre-deployment baseline after the rollback
* They did not notify users before initiating the rollback
* They did not re-run the CI pipeline against the reverted version

##### !end-options
##### !answer

* They did not verify that monitoring signals returned to their pre-deployment baseline after the rollback

##### !end-answer
##### !explanation

Executing a rollback is not the same as confirming recovery. After restoring a prior version, the team must check that the metrics that indicated the problem (error rates, latency, health checks, business signals) have returned to their normal pre-deployment levels. Skipping this verification step means an incident can appear closed while users are still being affected.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 965c9926-6d03-4f72-b06d-7fdf918cb400
* title: Problem Set: Deployment, CI/CD, & Automation
##### !question

A startup is deploying a low-risk dependency version bump across their fleet of application servers. They want a simple approach with no downtime and don't need an extended observation window. Which deployment strategy is the most practical fit?

##### !end-question
##### !options

* Blue/green deployment, because instant rollback is always worth the cost
* Gradual rollout, because all deployments should go through percentage-based stages
* Rolling deployment, because it replaces instances sequentially with low complexity and no downtime
* Canary release, because real user traffic should always validate changes before full rollout

##### !end-options
##### !answer

* Rolling deployment, because it replaces instances sequentially with low complexity and no downtime

##### !end-answer
##### !explanation

Rolling deployments are well-suited for routine, lower-risk changes. They replace instances one at a time (or in small groups), keeping the application available throughout with no downtime, and require no specialized traffic-splitting infrastructure or parallel environments. Blue/green is powerful but expensive to operate and most justified for high-stakes releases. Gradual rollouts and canary releases offer stronger risk controls but add meaningful operational complexity that isn't warranted for a routine dependency update.

##### !end-explanation
### !end-challenge
<!--prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 2vCpRmW8kJnTbxQsYeLgZa5dH0fUo3
* title: Problem Set: Deployment, CI/CD, & Automation
##### !question

You have a new release candidate and want a fast rollback path by keeping the previous version ready while you validate the new one with real traffic. Which strategy best fits?

##### !end-question
##### !options

* Rolling deployment, because it replaces instances one at a time and stops if a problem is detected
* Gradual rollout, because it expands the release in percentage tiers with explicit validation checkpoints
* Blue/green deployment, because a second full environment runs the new version while the previous environment stays live and idle
* Canary release, because it routes a small percentage of traffic to the new version for an extended observation window

##### !end-options
##### !answer

* Blue/green deployment, because a second full environment runs the new version while the previous environment stays live and idle

##### !end-answer
##### !explanation

**Blue/green deployment** is the strongest fit when the top priority is having the previous version immediately available for rollback. 

Rolling deployments can stop mid-rollout, but rolling back means re-deploying the old version, which takes time. Canary and gradual rollout strategies also require re-deploying to reverse course. Blue/green is the only strategy where the previous version is continuously running and immediately available as a rollback target throughout the validation window.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 7hKsNmBpRwYqLzXcTe4vJd1gF9aU0i
* title: Problem Set: Deployment, CI/CD, & Automation
##### !question

Which option best describes the main risk-control benefit of a gradual rollout?

##### !end-question
##### !options

* It eliminates the possibility of bugs reaching production by running more tests before each deployment
* It removes the need for a rollback plan because the rollout can be paused at any checkpoint
* It limits the number of users affected at each stage and allows the team to validate real-world behavior with data before expanding exposure further
* It reduces infrastructure costs by running only one environment throughout the deployment

##### !end-options
##### !answer

* It limits the number of users affected at each stage and allows the team to validate real-world behavior with data before expanding exposure further

##### !end-answer
##### !explanation

A gradual rollout releases a new version to an increasing percentage of users in deliberate stages, for example, 5% → 25% → 50% → 100%, with explicit validation checkpoints between each step. The primary risk-control benefit is **blast radius management**: if a problem surfaces, it affects only the users already in the rollout rather than the entire user base, buying the team time to pause, investigate, or reverse course before the issue spreads. 

Gradual rollouts do not eliminate bugs (option A) or remove the need for rollback planning (option B). They also typically require additional traffic-splitting infrastructure, meaning they can increase cost and complexity rather than reduce it (option D).

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: d2b5b8a2-d7f0-4d3d-a20c-2b102d0428cf
* title: Problem Set: Deployment, CI/CD, & Automation
##### !question

Shortly after a newly released recommendation engine goes live to all users, the on-call engineer notices a sharp increase in error rates. The team needs to stop the bleeding immediately while they investigate the root cause. Which of the following represents the fastest and lowest-risk response if the feature was built with a kill switch?

##### !end-question
##### !options

* Redeploy the previous version of the application using the release pipeline.
* Flip the feature flag off, routing all users back to the previous recommendation behavior without a deployment.
* Initiate a blue/green traffic switch back to the prior environment.
* Roll back the database to its state before the feature launched.

##### !end-options
##### !answer

* Flip the feature flag off, routing all users back to the previous recommendation behavior without a deployment.

##### !end-answer
##### !explanation

A kill switch is a feature flag whose purpose is emergency rollback. Because it's a configuration change rather than a deployment, it takes effect immediately with no pipeline to run and no infrastructure changes to coordinate. This is one of the primary reasons teams invest in feature flags: the ability to reverse a feature release in seconds without going through the full deployment process.

##### !end-explanation
### !end-challenge
<!--prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 80ac0465-16e3-4a97-9d35-5c995f6105c5
* title: Problem Set: Deployment, CI/CD, & Automation
##### !question

A startup begins using feature flags stored as simple environment variables set manually on each server. As the team grows and the number of flags increases, which of the following problems would a proper configuration management system most directly help solve?

##### !end-question
##### !options

* Ensuring that feature flags are compiled into the application binary for faster reads.
* Providing a central store with access controls, validation, and an audit trail of who changed which flag and when.
* Automatically removing flags from the codebase after they reach 100% rollout.
* Generating unit tests for every conditional branch introduced by a feature flag.

##### !end-options
##### !answer

* Providing a central store with access controls, validation, and an audit trail of who changed which flag and when.

##### !end-answer
##### !explanation

As feature flag usage scales, managing them through manually-set environment variables breaks down. A configuration management system addresses this by centralizing where flags live, restricting who can change them in production, validating that values are well-formed before they go live, and logging every change with author and timestamp. This applies the same rigor to configuration that CI/CD applies to code, which is important because a misconfigured flag can cause a production incident just like bad code in a deployment can.

##### !end-explanation
### !end-challenge
<!--prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 2730bdc1-8194-4e10-bd69-f992e36c57c1
* title: Problem Set: Deployment, CI/CD, & Automation
##### !question

A team is preparing to launch a major redesign of their mobile app's onboarding flow. The product manager wants to release it first to users who opted into beta testing, then to all new signups, and finally to the full user base, each stage happening weeks apart. The engineering lead says only one deployment is needed. Which approach makes this possible?

##### !end-question
##### !options

* Running three separate versions of the application simultaneously, each deployed to a different environment.
* Using a gradual rollout deployment strategy that automatically expands traffic over time.
* Shipping the new onboarding flow behind a feature flag that can be enabled for specific user segments through configuration changes.
* Scheduling three separate production deployments timed to each release stage.

##### !end-options
##### !answer

* Shipping the new onboarding flow behind a feature flag that can be enabled for specific user segments through configuration changes.

##### !end-answer
##### !explanation

Feature flags allow a team to ship code once and then control who sees a feature through configuration rather than through additional deployments. Enabling the flag for beta testers, then new signups, then everyone is a series of configuration changes: no new builds or deploys required. This is the core value of decoupling shipping code from releasing features: the technical operation (deploy) happens once, and the product decision (who sees the feature, when) is made separately through configuration.

##### !end-explanation
### !end-challenge
<!--prettier-ignore-end -->