# Automation: Infrastructure-as-Code

Core question: How do we make environments reproducible and auditable?

IAC as it relates to CI/CD and defining the pipeline through a configuration file/scripting
That means the pipeline is:
- Consistent: Every code change goes through the exact same sequence of build, test, and deployment steps, in the same order, using the same tooling.
- Auditable: Because the pipeline definition is version-controlled, teams can see exactly what the pipeline looked like when any given change was released. If a deployment process changes, that change is tracked.
- Independent of individuals: The pipeline runs the same way whether it was triggered by a senior engineer or a new team member, on a Tuesday morning or a Friday afternoon. Human variability is removed from the process.

Covers:
IaC as desired state + versioning
Reducing config drift
Repeatability & auditability

CFU Ideas:
The core idea of Infrastructure as Code (IaC) is to....?
Which problem does IaC most directly help reduce?
Why is version control important for infrastructure definitions?