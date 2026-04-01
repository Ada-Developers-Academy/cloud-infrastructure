# CI & CD: Why Pipelines Exist

## Learning Goals

- Define Continuous Integration, Continuous Delivery, and Continuous Deployment 
- Describe key benefits of Continuous Integration, Continuous Delivery, and Continuous Deployment and where they sit in a release pipeline

## Vocabulary and Synonyms

| Vocab | Definition | Synonyms | How to Use in a Sentence |
| --------- | --------- | -------- | --------- |
| Version Control | A system for tracking changes to code over time. It lets multiple developers work on the same project, see who changed what, and roll back to earlier versions if needed. Git is the most common example. | Source control, revision control | "After the bug was introduced, the team used version control to identify exactly which commit caused the issue and roll back to the last stable state." |
| Artifact Management | A way to store and manage the built versions of your application (not just the source code). An artifact might be a compiled app, a Docker image, or a packaged release file that's ready to deploy. | Artifact repository, package registry | "Our artifact management system stores a versioned Docker image for every successful build so we can redeploy any previous release without rebuilding from source." |
| Build Automation | Automatically turning raw source code into something runnable. This usually includes installing dependencies, compiling code, and running tests without a human doing it manually. | Automated build, build pipeline | "With build automation in place, every time a developer merges a pull request, the system installs dependencies, compiles the code, and runs tests without any manual steps." |
| Continuous Integration (CI) | A practice where developers frequently merge small code changes into a shared branch, and automated tests run every time to make sure nothing breaks. | CI, automated integration | "Since adopting continuous integration, the team catches merge conflicts and failing tests within minutes of a commit instead of discovering them days later." |
| Continuous Delivery | An extension of CI where code is always in a deployable state. After tests pass, the system prepares the app for release, but a human may still approve the final production deployment. | CD | "We use continuous delivery so that our staging environment always has a tested, release-ready build, and a team lead approves the final push to production." |
| Continuous Deployment | An extension of continuous delivery where code automatically deploys to production after passing all tests, with no manual approval step. | Fully automated deployment, continuous release | "With continuous deployment, a developer's change can go from merged pull request to live in production within minutes as long as all automated tests pass." |
| Pipeline Orchestration | The system that coordinates the steps in a CI/CD pipeline. It decides the order of actions like build → test → deploy and triggers each step automatically when code changes. | Pipeline coordination, workflow automation | "Our pipeline orchestration tool kicks off a build the moment code is pushed, then runs tests, and only triggers a deployment to staging if every test passes." |

## What are CI & CD?

We've established that a release pipeline is the sequence of steps code travels through on its way to production. But what keeps that pipeline running smoothly and automatically, rather than requiring engineers to manually kick off each stage? The answer is a set of practices known as CI/CD.

CI/CD stands for Continuous Integration, Continuous Delivery, and Continuous Deployment. These are a family of related practices that, taken together, describe how teams automate and standardize the journey from a merged pull request to a running production system. These practices build on one another: each piece picks up where the previous one leaves off and extends automation a step further down the release pipeline.

We've likely already encountered CI/CD by using a managed deployment platform. When we connect a GitHub repository to a deployment service like Render, we are able to choose a branch of our repository to track. When we push new code to the branch, the platform automatically triggers a redeploy. That automatic trigger is CI/CD at work!
- In a production engineering organization, the same principle applies, but with more steps, environments, and safeguards in between.

## Continuous Integration

Think about a group project in school where everyone works on their section independently and then tries to combine everything the night before it's due. The content might be good, but the formatting is inconsistent, some sections contradict others, and the introduction no longer matches the conclusion. The later you wait to integrate everyone's work, the messier the merge.

**Continuous Integration** (CI) is the software development answer to this problem. CI is the practice of merging code changes into a shared branch frequently, often multiple times per day, with automated builds and tests that run every time a PR opens or merge happens. The goal is to catch problems early, when they're small, cheap to fix, and easy to trace back to a specific change.

CI lives at the build and test stages of the release lifecycle and pipelines. There are a number of open source and cloud vendor provided products for CI servers that act as the "glue" of the CI system. CI systems typically use event driven architectures, where they listen for events from repos like "PR opened" or "PR merged". When they recieve an event, the system triggers whatever steps that a team has outlined through scripts for their build and test process.

When a developer opens a pull request or merges code, the CI system automatically:
1. Pulls the latest code from the repository
2. Installs dependencies
3. Compiles or packages the code if needed
4. Runs the test suite (unit tests, linting, static analysis)
5. Produces a build artifact if the tests pass, or stops and raises an alert if they don't

If any step fails, the pipeline stops and no artifact is created. The developer is notified so they can fix the issue before it can travel further through the pipeline.

### Key benefits

- **Early bug detection**: Bugs are caught right after they're introduced, when the fix is simple and fast.
- **Smaller, safer merges**: Frequent integration means less code changes at once, so conflicts and surprises are far less common.
- **A trusted artifact**: A passing CI run produces something the rest of the pipeline can rely on.

## Continuous Delivery

**Continuous Delivery** (CD) extends CI by automating the pipeline further through staging deployment, integration testing, and any other validation steps. This means our code is always in a state that *could* be deployed to production. The defining feature of Continuous Delivery is the word could: a human still makes the decision of when to actually release to production.

We can think of it like a restaurant's kitchen pass: food is cooked, plated, checked by the chef, and sitting under the heat lamp ready to go. It could go out immediately. But a server has to pick it up and carry it to the table. Continuous Delivery is the practice that keeps food on the kitchen pass. The decision of when to send it out remains with the team.

As we mentioned, CD picks up where CI leaves off, taking the artifact created by CI and moving it through the staging and pre-production validation stages. After a successful CI run:
1. The artifact is automatically deployed to the staging environment
2. Integration tests, smoke tests, and any additional automated checks run against staging
3. The pipeline marks the release as ready and notifies the team

At this point a human can review the results and if everything looks good,approve the production deployment, starting the next step in the release pipeline.

The staging deployment and its associated tests are the critical addition here. Code that passed unit tests in CI now gets validated against an environment that mirrors production, which catches a different class of bugs: issues with configuration, infrastructure compatibility, or interactions between services that unit tests can't surface, all before they can affect a real user.

### Key benefits

- **Reduced release risk**: By the time a human approves a production deploy, the change has already been tested in a realistic environment.
- **Always release-ready**: The team can deploy at any time because the pipeline is continuously preparing tested, validated artifacts.
- **Controlled release timing**: Becasue they are always release-ready, teams can coordinate releases with business needs, like an end-of-sprint window, a low-traffic period, or a planned announcement, without slowing down the automation work that prepares the release.

## Continuous Deployment

define Continuous Deployment specifically, key benefits, and where it fits into the release lifecycle with examples. Note how this is different from Continuous Delivery

Core question: What should be automated before anything hits production?

## How does CI/CD make Pipelines more repeatable

Pipelines as a repeatable path
Automated tests and checks / fast-feedback loops

## Summary


## Check for Understanding

CFU Ideas:
Continuous Integration (CI) primarily focuses on which of the following?
What’s the most accurate difference between Continuous Delivery and Continuous Deployment?
The biggest benefit of a CI/CD pipeline is that it...?