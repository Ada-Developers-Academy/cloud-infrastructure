# Problem Set: Deployment, CI/CD, & Automation

Suggested Questions:
A pull request is merged. Which sequence best represents a typical, safe path to production?
Which option best distinguishes CI, Continuous Delivery, and Continuous Deployment?
You have a new release candidate and want a fast rollback path by keeping the previous version ready while you validate the new one with real traffic. Which strategy best fits?
Which option best describes the main risk-control benefit of blue/green deployment?
A production deploy is causing errors. Which situation is most likely to make a rollback insufficient or risky?
Which answer best captures both the purpose of staging and a common reason it can give false confidence?
Your team keeps running into “it works in staging but not production” because environments drift over time. Which approach most directly addresses this problem?



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



--------------


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
* id: ca4c7337-0308-4a1f-827b-e748f67761e3
* title: Problem Set: Deployment, CI/CD, & Automation
##### !question

A fintech company's compliance team requires that a senior engineer sign off on every change before it goes live in production. The rest of their pipeline — building, testing, and staging deployment — is fully automated. Which CI/CD practice best describes this setup?

##### !end-question
##### !options

* Continuous Integration only, because nothing is deployed automatically
* Continuous Deployment, because all testing and staging steps are automated
* Continuous Delivery, because the pipeline automates validation but a human approves the production release
* Neither, because true CI/CD does not allow manual approval steps

##### !end-options
##### !answer

* Continuous Delivery, because the pipeline automates validation but a human approves the production release

##### !end-answer
##### !explanation

Continuous Delivery means code is always in a deployable state after passing automated checks, but the decision to release to production remains with a human. This is especially appropriate in regulated industries where compliance requires an approval step before changes reach end users.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->



------------------



<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: ecc5b399-e2b2-43b4-a130-38365f161b79
* title: Infrastructure-as-Code
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
* title: Infrastructure-as-Code
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