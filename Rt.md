3rd Section: Leadership & Org Contribution

B7

1. J&J AWS Migration Project
- Acted independently as the sole DevOps engineer on the project, taking full responsibility for all DevOps decisions and their outcomes without depending on others for direction.
- When blockers affected every environment, I took the initiative to design a dev-only Wiz bypass and an interim deployment workaround without waiting for escalation, so development and testing could continue.
- Led by example in handling security exceptions. Every workaround was limited in scope, clearly explained to the team, and never applied to production.
- Kept the team focused and confident during the UAT phase by turning blockers around quickly and communicating fixes clearly, so developers could concentrate on their deliverables.

2. IaC DevOps Capability
- Actively took part in the IaC DevOps capability, contributing to capability-building activities alongside my project responsibilities.
- Contributed to the drift validation use case, which helps detect differences between the infrastructure defined in code and the actual deployed state.
- Published a blog on Policy as Code, based on my HU intern phase work, to share IaC governance practices with the wider community.

B6L

1. J&J AWS Migration Project
- Took complete ownership of the project's DevOps requirements, including environment setup, production stabilization, pipeline fixes, cron job automation, and release scoping for each phase.
- Shared regular progress updates on issues, their impact, and the fixes required in the internal project group, which kept the team and stakeholders aligned on delivery status.
- Proactively supported developers by fixing Helm chart environment mappings that had been forcing manual script runs, which removed repetitive effort and restored an automated flow.
- Showed strong ownership of schedule-critical trade-offs, such as scoping the UAT release to IRP Core and keeping the Wiz bypass limited to dev. I clearly explained the reasoning and risks behind each decision.
- Guided the development team on proper branching practices and on how releases move through the pipeline, which improved coordination and release discipline across the team.
- Supported other project teams facing the same Wiz failure from the common client pipeline template by sharing my fix, which helped them unblock their own deployments.

2. Org Initiatives
- Contributed to business improvement beyond my assigned tasks by proposing a proper branching strategy, a new environment plan with testing defined for each stage, regression automation, and observability with Grafana and CloudWatch.
- Contributed to the IaC capability through the drift validation use case and the Policy as Code blog, adding practical assets the capability can reuse.

3. Recruitment & Training
- Served as an evaluator across 3 tracks for the HU batch of .NET developers converting to new tracks, assessing their technical readiness and understanding.
- Supported org-level talent development by giving fair and constructive evaluations that helped identify learners ready for project allocation.

4th Section: Role Expertise

B7

1. J&J AWS Migration Project (DevOps Competency)

- Fulfilled the complete DevOps role on the project, applying hands-on skills across AWS IAM, Kubernetes, Helm, Jenkins, JFrog, and Wiz to set up, stabilize, and automate environments.
- Set up and stabilized the production environment by resolving issues with assume-role permissions that blocked namespace access, recurring Helm upgrade failures, pod access to the Secret Provider Class, and image pull secret configuration.
- Built 5 cron jobs and reusable Helm chart templates covering monthly, quarterly, bi-yearly, yearly, and ad hoc schedules, and enhanced the daily and weekly scripts so data is fed into the database reliably.
- Diagnosed the version-calculation issue in the pipeline that caused wrong artifacts to deploy to QA and prod, and handled Wiz scan integration issues coming from the client's pipeline template.

2. Documentation
- Documented the issues, root causes, and fixes from production stabilization and the AWS migration, so the team could refer to them and resolve similar problems faster.
- Shared this knowledge with my team and with other teams affected by the same issues, which supported smooth project operations.

3. Learning
- Completed the AWS Certified Cloud Practitioner certification to strengthen my AWS fundamentals alongside my project work.
- Building new competencies in observability, through Grafana dashboards with CloudWatch, and in test automation, through Robot Framework regression testing.
- Shared knowledge with the wider community by publishing a blog on Policy as Code.

B6L

1. DevOps Proficiency
- Demonstrated AWS proficiency in production across IAM roles and assume-role access, Kubernetes workloads, secrets management, and CloudWatch.
- Rapidly adopted new tools as the project needed them, including Wiz security scanning, JFrog artifact management, and the Secret Provider Class integration, and applied them effectively under tight timelines.
- As an HU-trained resource with a foundation across multiple cloud platforms and most DevOps tools, I adapt quickly to each new project's tech stack and become productive in a short time.

2. Secondary Competencies
- Developing IaC governance skills through my work on Policy as Code and the drift validation use case in the IaC capability.
- Expanding into observability by building Grafana dashboards with CloudWatch as the data source for all microservices in the project.
- Building test automation skills by adding Robot Framework regression testing to the Jenkins pipeline.

3. Knowledge Transfer
- Ensured project continuity by documenting fixes and sharing regular updates in the internal project group, so knowledge is not limited to one person.
- Extended the Wiz fix to other teams facing the same issue, and shared knowledge more broadly through the Policy as Code blog.

4. Improvement in Primary Area
- Grew from fixing individual deployment issues to shaping release management for the project. This included recommending a branching strategy, designing a new environment between QA and prod, and planning which testing happens in each environment.
