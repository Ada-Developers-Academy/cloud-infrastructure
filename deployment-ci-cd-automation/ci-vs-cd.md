# CI & CD: Why Pipelines Exist

## Learning Goals

- Define Continuous Integration, Continuous Delivery, and Continuous Deployment 
- Describe key benefits of Continuous Integration, Continuous Delivery, and Continuous Deployment and where they sit in a release pipeline

## Vocabulary and Synonyms

| Vocab | Definition | Synonyms | How to Use in a Sentence |
| --------- | --------- | -------- | --------- |
| Artifact Management | A way to store and manage the built versions of an application (not just the source code). An artifact might be a compiled app, a Docker image, or a packaged release file that's ready to deploy. | Artifact repository, package registry | "Our artifact management system stores a versioned Docker image for every successful build so we can redeploy any previous release without rebuilding from source." |
| Build Automation | Automatically turning raw source code into something runnable. This usually includes installing dependencies, compiling code, and running tests without a human doing it manually. | Automated build, build pipeline | "With build automation in place, every time a developer merges a pull request, the system installs dependencies, compiles the code, and runs tests without any manual steps." |
| Pipeline Orchestration | The system that coordinates the steps in a CI/CD pipeline. It is configured to execute the order of actions like `build` → `test` → `deploy` and triggers each step automatically when code changes. | Pipeline coordination, workflow automation | "Our pipeline orchestration tool kicks off a build the moment code is pushed, then runs tests, and only triggers a deployment to staging if every test passes." |
| Rollback | Reverting a system back to a previous stable version when something goes wrong during deployment. | Revert, rollback deploy, release rollback | "When the new checkout feature caused a spike in errors, the team triggered a rollback to the previous stable release while they investigated the root cause." |

## What are CI & CD?

We've established that a release pipeline is the sequence of steps code travels through on its way to production. But what keeps that pipeline running smoothly and automatically, rather than requiring engineers to manually kick off each stage? The answer is a set of practices known as **CI/CD**.

CI/CD stands for Continuous Integration, Continuous Delivery, and Continuous Deployment. These are a family of related practices that, taken together, describe how teams automate and standardize the journey from a merged pull request to a running production system. These practices build on one another: each piece picks up where the previous one leaves off and extends automation a step further down the release pipeline.

When we connect a GitHub repository to a deployment platform like Render, we are able to choose a branch of our repository to track. When we push new code to the branch, the platform automatically triggers a redeploy. That automatic trigger is CI/CD at work!
- In a larger organization, the same principle applies, but with more steps, environments, and safeguards in between.

## Continuous Integration

Think about a group project where everyone works on their section independently and then tries to combine everything the night before it's due: the content might be good, but the formatting is inconsistent, some sections contradict others, and the introduction may no longer match the conclusion. The later we wait to integrate everyone's work, the messier the merge!

**Continuous Integration** (CI) is the software development answer to this problem. CI is the practice of merging code changes into a shared branch frequently, often multiple times per day, with automated builds and tests that run every time a PR opens or merge happens. The goal is to catch problems early, when they're small, cheap to fix, and easy to trace back to a specific change.

CI lives at the build and test stages of the release lifecycle and pipelines. CI systems typically use event driven architectures, where they listen for events from repos like "PR opened" or "PR merged". When they receive an event, the system triggers whatever steps that a team has outlined through scripts for their build, test, and artifact storing processes.

When a developer opens a pull request or merges code, the CI system automatically:
1. Pulls the latest code from the repository
2. Installs dependencies
3. Compiles or packages the code if needed
4. Runs the test suite (unit tests, linting, static analysis)
5. Produces a build artifact if the tests pass and sends it to whatever storage the team is using for artifact management, or stops and raises an alert if the tests fail

If any step fails, the pipeline stops and no artifact is created. The developer is notified so they can fix the issue before it can travel further through the pipeline.

### Key benefits

- **Early bug detection**: Bugs are caught right after they're introduced, when the fix is simple and fast.
- **Smaller, safer merges**: Frequent integration means less code changes at once, so conflicts and surprises are far less common.
- **A trusted artifact**: A passing CI run produces something the rest of the pipeline can rely on.

## Continuous Delivery

**Continuous Delivery** (CD) extends CI by automating the pipeline further through staging deployment, integration testing, and any other validation steps. This means our code is always in a state that *could* be deployed to production. The defining feature of Continuous Delivery is the word _'could'_: a human still makes the decision of when to push the release to production.

We can think of it like a restaurant's kitchen pass: food is cooked, plated, checked by the chef, and sitting under the heat lamp ready to go. The food could go out immediately, but a server has to pick it up and carry it to the table. Continuous Delivery is the practice that keeps food on the kitchen pass. The decision of when to send it out remains with the team.

As we mentioned, CD picks up where CI leaves off, taking the artifact created by CI and moving it through the staging and pre-production validation stages. A continuous delivery processes may look like:
1. The artifact from CI is automatically deployed to the staging environment
2. Integration tests, smoke tests, and any additional automated checks run against staging
3. The pipeline marks the release as ready and notifies the team

At this point a human can review the results and if everything looks good, approve the production deployment, starting the next step in the release pipeline!

The staging deployment and its associated tests are the critical addition here. Code that passed unit tests in CI now gets validated against an environment that mirrors production, which catches a different class of bugs: issues with configuration, infrastructure compatibility, or interactions between services that unit tests can't surface, all before they can affect a real user.

### Key benefits

- **Reduced release risk**: By the time a human approves a production deploy, the change has already been tested in a realistic environment.
- **Always release-ready**: The team can deploy at any time because the pipeline is continuously preparing tested, validated artifacts.
- **Controlled release timing**: Because they are always release-ready, teams can coordinate releases with business needs, like an end-of-sprint window, a low-traffic period, or a planned announcement, without slowing down the automation work that prepares the release.

## Continuous Deployment

**Continuous Deployment** takes the final manual step out of the picture. In a continuous deployment pipeline, every change that passes all automated checks is deployed to production automatically, with no human approval required. If the tests pass, users see the change.

This is the most automated point on the CI/CD spectrum, and it is the model used by many high-velocity consumer software teams. It is also the model that creates the most pressure on test coverage and monitoring. Aside from the initial code review for a pull request, no human reviews a change before it reaches users, so the automated checks become the last line of defense.

This is not a shortcut or a sign of less care; it's a model that requires more rigor in automated testing and monitoring precisely because the automation has to be trustworthy enough to act without asking permission. Teams that practice Continuous Deployment typically have extensive test suites, strong monitoring, and automated rollback to a prior build configured (which we'll talk about in a later lesson), because those systems are doing the job that a human reviewer does in a continuous delivery pipeline.

Continuous deployment covers the complete pipeline without manual gates:
1. CI runs: build, test, artifact creation.
2. CD runs: Artifact is automatically deployed to staging. Integration and acceptance tests run against staging.
3. If all checks pass, the artifact is automatically deployed to production. Monitoring watches for degraded error rates or latency and may trigger automated rollback to the prior version.

### How this differs from Continuous Delivery

This distinction trips up a lot of people because the names are so similar. The difference comes down to one thing: who or what triggers the production deploy.

| | Who deploys to production? | When? |
|---|---|---|
| **Continuous Delivery** | A human, after reviewing a pipeline result | On a schedule or deliberate decision |
| **Continuous Deployment** | The pipeline, automatically | Immediately after all checks pass |

Both practices use the same automated pipeline through build, test, and staging, the only difference is in the final step.

### Key benefits

- **Maximum velocity**: Teams can ship many small changes per day without coordination overhead.
- **Smaller deploys, smaller risk**: Small releases are easier to roll back and easier to debug when something goes wrong.
- **Real user feedback faster**: Changes reach production quickly, so teams learn from real usage faster.

### When Continuous Deployment is not the right fit

Continuous Deployment is powerful, but it's not a fit for every team or every context. It's worth pausing before adopting it when:
- Regulatory or compliance requirements mandate human review before changes go live (common in healthcare, finance, and government software)
- Test coverage is not comprehensive enough to be trusted as the sole gatekeeper of production quality
- Reverting back to a prior stable version (a rollback) is difficult or impossible, which can happen when deployments include changes with lasting side effects

In these cases, Continuous Delivery is often the better fit because of its deliberate human approval step.

## CI/CD Makes Pipelines Repeatable

Most of us have experienced a deployment that went sideways at some point. Not because the code was bad, but because of something in the process around it. Maybe a step was skipped under deadline pressure, a configuration was set differently than usual, or one team member deployed things slightly differently than another. The code itself was fine; the way it moved to production wasn't. The heart of the issue is often that we didn't have a clear process that ensured the same things happened every single time. 

When we talk about a repeatable pipeline, we mean one where the same inputs always produce the same outputs through the same validated steps, regardless of who wrote the code, what time of day it is, or how many changes are in the release. The reason engineering teams invest in CI, CD, and Continuous Deployment isn't just to automate individual steps, it's to build a pipeline that behaves the same way every single time code moves through it. That consistency is what makes software releases predictable and trustworthy instead of nerve-wracking.

### Pipelines as a Repeatable Path

Pipeline orchestrators are the systems we use to organize the steps of our CI/CD pipelines. Events trigger the pipeline, and the orchestrator is generally responsible for feeding in the result of one step into the next step and reporting statuses. There are a number of open source and cloud vendor provided products aimed at the individual steps in a CI/CD process as well as at the orchestration layer. 

When we deploy to a platform like Render or Heroku by pushing to a connected GitHub branch, we're experiencing one of the simplest forms of a repeatable pipeline: the same trigger always causes the same result. But in most production environments, a lot more needs to happen between "code is merged" and "users see the change," and all of those steps need to be just as consistent as the initial trigger.

A CI/CD pipeline makes this possible by encoding the release process as configuration, typically a file that lives in the same repository as the application code itself. Rather than a checklist someone might follow (or skip), the process is defined once and executed the same way automatically, every time a change moves through it. This shifts the release process from something that lives in people's heads to something that lives in version control.

What that means in practice:
- A junior developer's first contribution and a staff engineer's fiftieth ship through the exact same steps
- A release at 2pm on a Wednesday and a hotfix at 11pm on a Friday go through the same checks
- If the pipeline definition changes, such as a new test suite being added, a deployment step is updated. That change is tracked in version control just like code, and applies to every future release

The last point is quietly significant. Manual deployment processes tend to drift over time: a step gets added here, a shortcut gets taken there, and six months later nobody is quite sure what the "official" process actually is. A pipeline defined in code doesn't drift unless someone deliberately changes it, and when it does change, there's a record of when, why, and by whom. 

It's also worth naming what repeatability prevents. As we've brought up, a large proportion of production incidents can be traced back to a manual step done incorrectly or inconsistently. Not bugs in the code, but mistakes in the process of getting the code to users. A well-designed CI/CD pipeline doesn't just automate those steps; it makes skipping or bypassing them structurally difficult. Code that doesn't pass the required checks doesn't advance. The pipeline is the gatekeeper, and it applies the same standard to every change without exception.

### Automated Tests and Checks: Fast Feedback Loops

Our release pipeline is the specific path we have created for our release lifecycle, and automated tests and checks are what make each step on that path meaningful. Rather than a single validation pass at the end of the process, CI/CD pipelines layer multiple kinds of checks throughout, each designed to surface a different class of problem as early as possible.

A typical pipeline includes several kinds of automated validation:
- Unit tests run immediately after the build and verify that individual pieces of code behave correctly. They run fast, usually in seconds or minutes, and provide the earliest signal that something is wrong.
- Linting and static analysis scan for code style issues, potential bugs, and common security vulnerabilities without executing the code at all.
- Integration tests often run after the code has been deployed to a staging environment, where the full application is assembled. These catch problems that unit tests typically can't see because they only surface when different parts of the system interact.
- Smoke tests run right after a deployment to confirm that the application actually started and that core functionality responds as expected.

The principle tying all of these together is fast feedback: catch problems as early in the pipeline as possible, while they're still cheap and fast to fix.

Here's why that ordering matters: imagine a bug gets introduced in a pull request. 
- If a unit test catches it two minutes after the developer opens the PR, the fix is straightforward, they still have complete context on the change, the problem is isolated, and nothing else has been built on top of it. 
- If that same bug slips past and is caught by a manual review at the end of the sprint, fixing it means context-switching back to work that's days old. 
- If it reaches production, the fix now includes triaging an incident, communicating with users, and potentially a rollback to a prior version.

The same bug, with three very different costs, determined entirely by where in the pipeline it was caught. This is why CI specifically emphasizes running tests on every pull request and merge rather than on a schedule or before major releases. The goal isn't just to catch bugs; it's to catch them as close to their origin as possible, before they compound with other changes or travel further through the system.

Fast feedback also changes how teams feel about their pipeline. A check that fails in under five minutes and tells us exactly what broke is useful. A validation step that takes an hour and returns a cryptic error is something teams learn to dread and look for ways around. Well-designed automated checks should feel like a helpful signal, not a punishment, and that experience is largely a function of how fast and specific the feedback is.

Repeatability and fast feedback aren't separate benefits of CI/CD, they reinforce each other. A repeatable pipeline gives automated checks a consistent environment to run in. Fast-feedback checks make the repeatable pipeline worth trusting. Together, they replace the anxiety of "I hope this deploy goes okay" with something much closer to confidence.

## Summary

At its core, CI/CD is about making software releases boring in the best possible way: predictable, automated, and consistent rather than nerve-wracking and manual. 

**Continuous Integration** kicks things off: developers merge changes often rather than stockpiling them, and every merge triggers an automated sequence that builds, tests, and packages the code or stops and raises an alert if something breaks. Running these checks immediately after each change keeps feedback tight, a developer still has full context on what they just wrote, and the fix is straightforward before other changes have been layered on top.

**Continuous Delivery** picks up from a passing CI run and automates the path through staging, where a production-like environment can surface problems that unit tests tend to miss, like mismatched configuration, unexpected service interactions, or infrastructure incompatibilities. The output is a release that's been thoroughly validated and is ready to go live whenever the team decides, with a human making that final call. 

**Continuous Deployment** takes human approval out of the equation entirely, deploying automatically once all checks pass. The tradeoff is that the automated checks have to be comprehensive enough to act as the sole gatekeeper: there's no human review between a passing test suite and real users. Teams that have compliance obligations, incomplete test coverage, or deployments with lasting side effects that are difficult to undo are generally better served by keeping deployment manual.

Underneath all three practices is the idea that a release process encoded in a file is fundamentally more reliable than one that exists as institutional knowledge. Pipeline configuration lives in version control alongside the application, so every release follows the same path, shortcuts are blocked rather than just discouraged, and changes to the process are reviewed and tracked just like any other code change. 
- This is what makes CI/CD valuable beyond the individual automation wins at each stage. It shifts deployments from something that requires careful coordination and institutional knowledge to something a new team member can trigger on their first week with full confidence that the same safeguards apply.

## Check for Understanding

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: c0e71571-85f4-49ae-ac2e-ca5e422993ea
* title: CI & CD: Why Pipelines Exist
##### !question

A development team notices that when multiple engineers merge their work at the end of a two-week sprint, integration is chaotic and bugs are hard to trace back to a specific change. Which practice most directly addresses this problem?

##### !end-question
##### !options

* Continuous Deployment, because it removes human approval gates and ships code faster
* Continuous Integration, because it encourages frequent merges with automated builds and tests that catch problems early
* Artifact management, because storing versioned builds makes it easier to identify which release introduced the bug
* Continuous Delivery, because it keeps code in a permanently deployable state at all times

##### !end-options
##### !answer

* Continuous Integration, because it encourages frequent merges with automated builds and tests that catch problems early

##### !end-answer
##### !explanation

Continuous Integration addresses the "big bang merge" problem by encouraging developers to merge small changes frequently. Automated builds and tests run on each merge, surfacing conflicts and bugs while they're still isolated and easy to fix, before other changes have been layered on top.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: c497e13a-e91b-4c41-9ca0-a9ee8dd80ebf
* title: CI & CD: Why Pipelines Exist
##### !question

Which of the following best describes what makes a CI/CD pipeline more reliable than a manual release checklist over time?

##### !end-question
##### !options

* Pipelines are faster than manual processes, which reduces the window of risk during a deployment
* Pipeline configuration lives in version control alongside application code, so the process is consistent, auditable, and can't silently drift
* Pipelines eliminate the need for staging environments because automated tests can fully replicate production conditions
* Pipelines allow engineers to skip individual steps when they are confident a change is low risk

##### !end-options
##### !answer

* Pipeline configuration lives in version control alongside application code, so the process is consistent, auditable, and can't silently drift

##### !end-answer
##### !explanation

Manual checklists tend to drift over time — steps get added informally, shortcuts get taken, and the "official" process becomes unclear. When a pipeline is defined as code and stored in version control, every release follows the same path, changes to the process are reviewed and tracked, and bypassing steps is structurally prevented rather than just discouraged.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: 84b9663e-9e34-4dff-8c9f-35841d5a654d
* title: CI & CD: Why Pipelines Exist
##### !question

A consumer app team ships dozens of small changes each day. Every change that passes their full automated test suite and staging checks is immediately released to users without any engineer reviewing or approving the deploy. One morning, a subtle bug slips past the automated checks and reaches production within minutes of being merged. Which practice does this scenario describe, and what tradeoff does the incident highlight?

##### !end-question
##### !options

* Continuous Delivery — the tradeoff is that staging environments don't catch all bugs
* Continuous Integration — the tradeoff is that merging frequently increases the chance of introducing bugs
* Continuous Deployment — the tradeoff is that automated checks serve as the sole gatekeeper, so gaps in test coverage can reach users quickly
* Artifact management — the tradeoff is that versioned builds are harder to roll back when many changes are released daily

##### !end-options
##### !answer

* Continuous Deployment — the tradeoff is that automated checks serve as the sole gatekeeper, so gaps in test coverage can reach users quickly

##### !end-answer
##### !explanation

Continuous Deployment removes the human approval step entirely, deploying automatically once all checks pass. This enables high velocity but places full responsibility on the automated test suite. When coverage has gaps, there is no human review to catch what the tests missed before users are affected.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->