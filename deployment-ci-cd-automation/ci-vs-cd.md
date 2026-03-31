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

You may have already seen this in action. If you've used a platform like Render or Netlify and noticed that pushing to GitHub automatically triggered a new deploy, you were experiencing a simple version of CI/CD. What we'll explore in this section is what that looks like at a larger scale, with multiple environments, more validation steps, and higher stakes.

We've likely already encountered CI/CD by using a managed deployment platform. When we connect a GitHub repository to a deployment service like Render, we are able to choose a branch of our repository to track. When we push new code to the branch, the platform automatically triggers a redeploy. That automatic trigger is CI/CD at work! 
- In a production engineering organization, the same principle applies, but with far more steps, environments, and safeguards in between.

## Continuous Integration

define Continuous Integration specifically, key benefits, and where it fits into the release lifecycle with examples

## Continuous Delivery

define Continuous Delivery specifically, key benefits, and where it fits into the release lifecycle with examples

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