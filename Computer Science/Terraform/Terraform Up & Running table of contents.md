# Table of Contents
[[1 Why Terraform]]
- An overview of infrastructure as code tools:
	- Configuration management, server templating, orchestration, provisioning tools
- The benefits of infrastructure as code
- How to combine tools such as Terraform, Packer, Docker, Ansible, and Kubernetes   

[[2 Getting Started with Terraform]]
- Terraform Install
- Overview of Terraform syntax
- Terraform CLI tool
- Deploying resources(single server, web server, cluster of web servers, load balancer)
- Clean up resources we've created  

[[3 How to Manage Terraform State]]
- What is Terraform State?
	- How do we store state so that multiple team members can access it?
	- How to lock state files to prevent race conditions?
	- How to manage secrete with Terraform?
	- How to isolate state files to limit the damage from errors?
- Terraform workspaces
- Best-practices for file and folder layout for Terraform projects.
- Read-only state

[[4 How to Create Reusable Infrastructure with Terraform Modules]]
- Modules
- Configurable modules with inputs and outputs
- Local values
- Versioned modules
- Defining Reusable, configurable pieces of infrastructure with modules

[[5 Terraform Tips and Tricks - Loops, If-Statements, Deployment, and Gotchas]]
- Loops, conditionals, and expressions
- Built-in functions
- Zero-downtime deployment
- Common Terraform gotchas and pitfalls
	- Count and for_each limitations
	- Zero-downtime deployment gotchas
	- How valid plans can fail
	- Refactoring problems

[[6 Production-Grade Terraform Code]]
- Production-grade infrastructure checklist
- Building Terraform modules for production
	- Small modules
	- Composable modules
	- Testable modules
	- Releasable modules
- Terraform Registry
- Terraform escape hatches

[[7 How to Test Terraform Code]]
- Manual tests
- Sandbox environments
- Automated tests
- Unit tests
- Integration tests
- End-to-end tests
- Dependency injection
- Running tests in parallel
- The test pyramid
- Static analysis
- Property checking

[[8 How to Use Terraform as a Team]]
- How to adopt Terraform as a team
- Workflow for deploying application code
- Workflow for deploying infrastructure code
- Version control
- Code review and coding guidelines
- CI/CD for Terraform
- The deployment process