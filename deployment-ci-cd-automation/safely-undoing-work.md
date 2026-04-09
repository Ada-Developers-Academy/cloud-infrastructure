# Rollbacks, Roll-forwards, and Safely Undoing Work

## Learning Goals

By the end of this lesson, students should be able to:

- Describe common signals that indicate a deployment has introduced a problem and a response is needed.
- Explain rollback and roll-forward as recovery strategies, and identify when each is appropriate.
- Explain the role of monitoring and verification in triggering and confirming a recovery response.
- Describe why certain changes are significantly harder or impossible to fully reverse.

## Vocabulary and Synonyms

| Vocab | Definition | Synonyms | How to Use in a Sentence |
| --------- | --------- | -------- | --------- |
| Rollback | Reverting a system to a previously deployed, known-good version after a release has introduced problems. | Revert, restore | "When error rates spiked after the release, the team initiated a rollback to the previous version while they investigated the cause." |
| Roll-forward | Responding to a bad deploy by developing and deploying a fix to the broken version, rather than reverting to the prior one. | Hotfix, forward fix | "Because the deployment included a database migration, the team decided to roll forward with a patch rather than risk reverting the schema." |
| Monitoring Gate | A defined threshold or condition checked automatically after a deployment that determines whether the release is healthy or triggers a rollback. | Automated rollback trigger, deployment health check | "The team configured a monitoring gate to automatically roll back if the error rate exceeded 2% within five minutes of a deploy." |

## When Do We Need to Rollback?

We've spent several lessons looking at how teams make software releases more reliable: building and testing code automatically, validating changes in staging before production, and using infrastructure-as-code to keep environments consistent. These practices meaningfully reduce the chance of a bad deploy, but they don't reduce it to zero. Production traffic loads have a way of surfacing problems that no staging environment predicted!

When something does go wrong after a release, a team is suddenly solving a different kind of problem than they face day-to-day. Writing code and fixing bugs is familiar territory; diagnosing a live system that's actively failing for real users while a clock ticks is not. Having a plan, and knowing when to trigger it, matters as much as knowing what to do.

The first part of that plan is detection: recognizing that something has gone wrong quickly enough to limit the damage. That recognition depends on two things being true: 
- First, there needs to be a monitoring system watching for problems.
- Second, the team needs to know what the system looked like *before* the deploy, because "something went wrong" is really "something changed in a bad direction." Without a baseline, we can't see the direction.

Later in the curriculum we'll talk about tooling for observation, for now let's take a look at common signals that teams monitor to check if a deployment has introduced a problem:
- **Error rate increase**: The application's error rate rises meaningfully above its pre-deployment baseline shortly after a release.
- **Latency degradation**: Requests that normally complete quickly are now slow, often causing cascading timeouts in other parts of the system.
- **Failed health checks**: Most platforms automatically check whether running instances are responsive. When instances start failing those checks after a deploy, the platform may already be routing traffic away from them.
- **Business metric drops**: Conversion rates, payment completions, or login success rates fall unexpectedly, sometimes before infrastructure metrics show anything unusual.
- **User-visible breakage**: Users report broken features before monitoring even picks it up.

A useful habit to develop is checking these signals *immediately* after every deploy, not just when something obviously looks wrong. Many teams build a post-deploy checklist or automation that can pull up the error rate dashboard, check latency, and look at a key business metric. A problem caught two minutes after a deploy is far cheaper than one caught two hours later.

## Rollback Mechanics

### What Is a Rollback?

Let's think about how version control works with code: if a commit introduces a bug, we don't have to rewrite history. We can check out a prior commit and we're back to the state the code was in before the problem. A rollback applies the same idea to a running production system: we revert to the version of the application that was running before the problematic release.

In practice, that means re-deploying a previous build artifact. This is why artifact versioning, which we mentioned in the CI/CD lesson, matters beyond just keeping things organized: without a library of prior build outputs to pull from, "rollback to the previous version" becomes much harder or time consuming than it sounds.

On managed deployment platforms, this process might be a button or a one-line command if the platform maintains a deployment history and handles the mechanics of restoring a prior version. One of the operational benefits of managed platforms is that this safety net often comes built in.

In a production engineering environment with a custom-built pipeline, the concept is the same but the team is responsible for the mechanics:
1. Identify the artifact version that was running before the bad deploy.
2. Re-deploy that artifact to the production environment.
3. Watch monitoring signals until they confirm the system has returned to a healthy state.
4. Investigate the root cause in a non-production environment, not in production under live user pressure.

That last step is worth naming explicitly. A core purpose of rolling back is to *buy time*. Once users are no longer experiencing the broken version, the pressure drops and the team can think clearly about what went wrong without an active incident forcing rushed decisions.

### Rollback vs. Roll-forward

When a bad deploy is caught, there are two broad recovery options: go backward (rollback) or go forward (roll-forward). A roll-forward means developing a fix for the problem and deploying that fix as a new release, rather than reverting to the prior version. These aren't just tactical choices, they reflect a judgment about which path back to a working system is actually safer and faster given the specifics of the situation.

Imagine a team has just deployed a release that's causing checkout failures on an e-commerce platform. They have two options:
- **Option A — Rollback**: Re-deploy yesterday's artifact. Checkout starts working again immediately. The team investigates the bug with no active user impact. Fix is developed, tested, and released properly. 
- **Option B — Roll-forward**: The team writes a fix, pushes it through the pipeline, and gets a new version to production. This takes longer, but users get the most current version of the application.

In this scenario, Option A is probably a good fit. But what if we change the scenario slightly? What if this release also ran a database migration that restructured the orders table? Now rolling back the application code means yesterday's code is running against today's schema, a schema it was never designed for. The application might break in new ways, possibly worse than the original problem. Suddenly Option B looks much more attractive, even if it takes longer.

**Rollback tends to be the right choice when:**
- The previous version is known to be stable and the problem is clearly in the new release.
- The fix isn't trivial to develop and test quickly under incident pressure.
- Users are actively impacted and restoring service fast is the priority.
- The deployment only changed application code, with no persistent side effects that would break the old version if it ran now (more on this shortly).

**Roll-forward tends to be the right choice when:**
- The deployment included a change that isn't compatible with the previous version of the application.
- The fix is small, well-understood, and can be written, tested, and deployed quickly with confidence.
- The previous version has known problems that make returning to it undesirable.

A useful mental model is that rollback prioritizes speed and certainty: we know the old version worked. Roll-forward prioritizes forward progress when reverting is complicated or costly. In practice, experienced teams develop an instinct for which to reach for, and the decision is often made in the first few minutes of an incident.

## Why Are Some Changes Harder to Undo?

Reverting application code is usually straightforward: we have the previous artifact, we deploy it, we're done. The complication arises when a deployment changes more than just the code. Database schema migrations and data transformations affect the persistent state of the system, and that state doesn't automatically revert when code does.

### The Schema Migration Problem

Consider a deployment that does two things: updates the application code *and* runs a migration that adds a new required column to a database table. If something goes wrong and we roll back the code, the old application now runs against a database schema that has an extra column it doesn't know about. Depending on how the old code was written, it might tolerate that gracefully, or it might fail on every query.

Now consider a slightly different scenario: the migration didn't just *add* a column, it *dropped* one that the old code depended on. Rolling back the code now leaves the old application querying for a column that no longer exists. Recovery in this case isn't just a re-deploy; it requires a separate migration to restore the dropped column, assuming the data was preserved somewhere to restore from.

This asymmetry is the core of the problem. Code lives in version control and can be swapped freely. Schema and data changes persist in the database and don't revert automatically.

Schema changes fall into two rough categories by how reversible they are:

- **Additive changes** — Updates like adding a new table, adding a nullable column, or adding an index are generally safe to leave in place when rolling back code. The old application simply ignores what it doesn't know about.
- **Destructive changes** — Changes such as dropping a column, renaming a column, changing a data type, or deleting rows may be difficult or impossible to reverse once they've run against a live production database, especially if the original data wasn't preserved before the migration ran.

### Data Transformations

A related challenge is data transformation: migrations that don't just change the structure of the database but change the values already stored in it. Say a migration converts all stored dates from one timezone to UTC, or reformats phone numbers from "555-867-5309" to "+15558675309". Rolling back the application code doesn't reformat those numbers back. The old code now reads data in a format it wasn't designed for, and every phone number in the system might render incorrectly or fail validation.

Depending on how the transformation was written, reversing it may or may not be feasible. If the original values were preserved in a backup or in a separate column before the transformation ran, recovery is possible. If not, the only realistic path forward is a roll-forward: write a version of the application that handles the new format, or write a new migration that reverses the transformation.

### The Expand-Contract Pattern

Teams that deploy frequently develop patterns specifically to manage this risk. One common approach is the **expand-contract migration**, which breaks a schema change into separate, smaller deployments that are each safe to reverse on their own.

Consider a team that needs to rename a column from `user_name` to `display_name`. Doing this in one migration and one code deploy is risky: rolling back the code while the column is already renamed breaks the old queries immediately. The expand-contract pattern handles it differently:

1. **Deploy 1 — Expand**: Add the new `display_name` column alongside the existing `user_name` column. Update the application to write to both columns and read from `display_name`. If this deploy needs to be rolled back, the database still has `user_name` and the old code still works.

2. **Deploy 2 — Migrate**: Backfill `display_name` with the values from `user_name` for any rows that haven't been updated yet. Both columns now have the same data.

3. **Deploy 3 — Contract**: Remove the old `user_name` column. By this point the new code has been stable for at least one full deploy cycle, confidence is high, and the old column is genuinely no longer needed.

Each deployment in this sequence is individually safer to roll back, because at each step the schema is compatible with *both* the previous and current version of the application code. No single deployment puts the team in a position where reverting the code would break the database layer.

The more a deployment changes the state of the system beyond the application code itself, the less reliable rollback becomes as a recovery option. This is one of the reasons experienced teams are careful about bundling large migrations with significant code changes, and why roll-forward is often the right call when they do.

### Verification and Monitoring Gates

Executing a rollback doesn't mean the incident is over. After restoring the previous version, the team needs to confirm that recovery actually worked. This means watching the same signals that indicated the problem in the first place: error rates, latency, health checks, and business metrics returning to their pre-deployment baselines.

Many CI/CD systems can automate the early part of this loop through **monitoring gates**: defined thresholds that the pipeline checks automatically after each deployment. If error rates exceed a configured limit within a window of time after a deploy, the gate fails and the system initiates a rollback automatically, without waiting for a human to notice and act. 
- This is especially useful in environments where releases ship frequently, since automated gates can respond in seconds while a human might not see the alert for several minutes.

Monitoring gates work in both directions: they can trigger automatic rollback when a deploy looks bad, and confirm that recovery is complete once it's been performed. Without this verification step, it's easy for a team to assume the incident is resolved while a problem is still affecting users.

## Summary

When a deployment causes problems in production, we typically have two choices: go backward or go forward. 
- Rolling back means re-deploying a prior build artifact to restore the system to a known-good state. Its primary value is speed and certainty, giving the team room to investigate without an active outage increasing pressure on them. 
- Roll-forward instead ships a corrective release, which becomes the better path when the deployment touched persistent state that the previous code can no longer safely read. A migration that restructured how data is stored, for instance, may leave the old application unable to function correctly even if the code itself is restored. In those cases, fixing forward is often cleaner than trying to unwind. 
 
The right call depends on what the deployment actually changed, and experienced teams develop an instinct for that judgment quickly.

A deeper challenge developers face is the asymmetry between code and data. "Undo" is a concept that applies cleanly to code but only partially to persistent state. Application artifacts live in a versioned library and can be swapped freely in either direction. Database schemas and stored values don't revert when code does; a transformation that reformats records persists in the database regardless of what happens to the application layer. 
- This is the problem the expand-contract pattern addresses, by spreading a risky schema change across multiple smaller deployments that each remain compatible with both the current and previous version of the application. No single release creates a situation where rolling back the code would break the data layer. 

Monitoring gates close the loop at deploy time. They automatically detect degraded signals and trigger recovery before a human might notice, and then confirm that recovery actually worked before the incident is considered closed.

## Check for Understanding

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: aB3mK9xTq2vLpR7nWcYd4eJf8h
* title: Rollbacks, Roll-forwards, and Safely Undoing Work
##### !question

A team pushes a new release to their web application and notices a spike in failed login attempts five minutes later. They decide to restore the version that was running before the push. Which of the following best describes what they are doing?

##### !end-question
##### !options

* Deploying a hotfix to address the authentication bug directly
* Re-running their staging test suite against the production environment
* Re-deploying a prior, known-working build artifact to replace the broken release
* Deleting the new release from version control to prevent further use

##### !end-options
##### !answer

* Re-deploying a prior, known-working build artifact to replace the broken release

##### !end-answer
##### !explanation

Re-deploying a prior, known-working build artifact to replace the broken release means we are performing a rollback. A rollback does not involve fixing the bug directly, removing code from version control, or re-running tests. It swaps the running application back to a state that was known to work before the problematic release.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: rN5wZ1sDjY8bXkPmCuH3tLaG6q
* title: Rollbacks, Roll-forwards, and Safely Undoing Work
##### !question

A delivery platform releases an update that both modifies order routing logic and runs a database migration that removes a deprecated column the old code relied on. An error is detected immediately after deployment. Why would rolling back the application code likely be insufficient to resolve the problem?

##### !end-question
##### !options

* The old code was never stored in an artifact library, so there is nothing to re-deploy
* Rolling back only affects front-end assets, not server-side logic
* The database column the old code depends on no longer exists, so the reverted code would still fail
* Rollbacks can only be triggered by automated monitoring gates, not manually

##### !end-options
##### !answer

* The database column the old code depends on no longer exists, so the reverted code would still fail

##### !end-answer
##### !explanation

Rolling back application code restores the previous version of the software, but it does not undo changes to a database schema. If the migration removed a column that the old code queries, re-deploying the old code leaves it trying to access data that no longer exists, potentially causing new failures on top of the original ones. This is a core reason why destructive schema changes make rollbacks risky.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: hJ6oR4zWnFk1cMxPyA8eBsLtDu
* title: Rollbacks, Roll-forwards, and Safely Undoing Work
##### !question

An engineering team is debating whether to roll back or roll forward after a bad deploy. The fix is small and well-understood, but the release also ran a migration that reformatted stored phone numbers into a new international format. The old application code expects the original format. Which factor most strongly suggests they should roll forward rather than roll back?

##### !end-question
##### !options

* The fix is small, which means it can be deployed quickly
* The old code cannot safely read the data in its new format, making reverting to it likely to introduce new failures
* The team has a post-deploy checklist they haven't finished yet
* Rollbacks are only appropriate when the previous version is unknown

##### !end-options
##### !answer

* The old code cannot safely read the data in its new format, making reverting to it likely to introduce new failures

##### !end-answer
##### !explanation

When a migration transforms stored data into a format that older application code was not designed to handle, rolling back the code does not restore the data to its original shape; the values in the database remain in the new format. The reverted application would then read data it can't process correctly. This incompatibility between the old code and the transformed data is the primary signal that rolling forward with a fix is the safer path.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->

<!-- prettier-ignore-start -->
### !challenge
* type: multiple-choice
* id: kL8dH1uAbQ3yRfMoWzEtViNcJs
* title: Rollbacks, Roll-forwards, and Safely Undoing Work
##### !question

A team needs to rename a database column used across their application. Rather than doing it in one deployment, they split it into three releases: first adding the new column alongside the old one, then backfilling data into it, and finally removing the old column once the new code is stable. What problem is this approach primarily designed to solve?

##### !end-question
##### !options

* Keeping CI/CD pipelines from running schema migrations automatically
* Ensuring each individual deployment remains safely reversible without breaking the data layer
* Reducing the total number of database queries the application makes
* Preventing engineers from needing to write migration scripts

##### !end-options
##### !answer

* Ensuring each individual deployment remains safely reversible without breaking the data layer

##### !end-answer
##### !explanation

This is the expand-contract migration pattern. By spreading a risky schema change across multiple smaller deployments, each release stays compatible with both the previous and the current version of the application. No single deployment puts the team in a position where rolling back the code would cause the database layer to break. The old column remains available as a fallback until the new code is fully stable and trusted.

##### !end-explanation
### !end-challenge
<!-- prettier-ignore-end -->