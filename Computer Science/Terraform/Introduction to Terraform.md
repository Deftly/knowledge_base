# What is Terraform?
- Terraform is an open-source infrastructure as code tool which lets you build, change, and version cloud and on-prem resources safely and efficiently.  

# Challenges with Provisioning
## Provisioning Day 1
- How do we go from running nothing to running something

## Provisioning Day 2+
- Now we have an existing set of infrastructure but we want to evolve it over time as we add new services and remove services.  

# How Terraform Handles these Problems
- It allows you to declaratively define in a set of Terraform config files what you want your infrastructure to look like. 
	- So in something that is very simple and human readable you will describe your overall topology.  

![[Intro to Terraform Asset 1.png]]  

- Infrastructure can have many moving pieces, in the above example we have a VPC, on top of a security group, and within that environment a set of VMs, and overlaid on top of that you have a load balancer. 
- You can imagine how this graph would extend over time as different pieces are added and different amounts of complexity to it incrementally as the need arises.
- While we have displayed the topology in a graphical way, the way it's captured within the Terraform config is very lightweight and simply described high level resources. 
	- We describe the VPC resource and the fields required to create that element of our infrastructure and as we define other elements we can reference other resources.  

# Terraform Workflow
## Terraform Refresh
- This reconciles what Terraform thinks the world looks like(Terraform View) with the real world. It does this by querying our different infrastructure providers(VMware, AWS, Azure) to see what is actually running.  

## Terraform Plan
- This is where Terraform figures out what it needs to do to reconcile the differences between what is actually running and the desired state which is in our Terraform config.  

## Terraform Apply
- The last phase where we start with the plan and execute it against the real world. In doing this it will figure out the right order in which the resources need to be provisioned and where it has the opportunity to create things in parallel and where it needs to be sequential.  

## Day 2+ Evolution of Infrastructure
- We change our Terraform configuration by adding the definitions of the new elements and we rerun the exact same workflow.
- With the refresh Terraform will update it's view, and realize which resources are already present. Then when we run plan it will tell us which elements already exist in the desired state and identify which elements or changes needed to be done to reach the desired state. Finally we run apply and it adds and removes whatever is necessary to create the desired state.
- With this workflow our day 1 experience is exactly the same as our day2+ experience and that's important because this is a process we will keep doing for possible a very long time.  

## Day N
- There may come a day where we no longer need our infrastructure, maybe it is just a staging environment or a test environment. We then have the option to run a destroy which is basically a specialized version of apply which creates a destroy plan to remove all the elements that exist. 

---

# Terraform's Architecture
## Terraform Core
- There are 2 major components to Terraform's architecture. The first being the Terraform Core which is fed the Terraform config provided by the user and the state of the world which is managed by Terraform. 
	- Given the state and the Terraform config the Core is responsible for creating the graph of our different resources, how those resources relate to each other, what needs to be created, updated, destroyed, all the essential life cycle management.  

## Terraform Providers
- The other major components are the many different providers that Terraform supports. It is through providers that Terraform connects to the rest of the world. 
	- Can be cloud providers(AWS, Azure, GCP), or on-prem infrastructure(Openstack, VMware). But it's not just limited to infrastructure as a service, it can also manage platforms as a service(Kubernetes, OpenShift) and even software as a service(Datadog, Fastly, Github Teams).  

- Everything for the IaaS, to PaaS, and SaaS are all important for the overall delivery and what we want to do with Terraform is get to a single unified workflow.
	- What we want to do this is to have anything that has an API and has a life cycle associated with it, enable it to be a provider so that Terraform can be used to manage our end to end delivery.  

### Continue Reading
[[Terraform Up & Running table of contents]]
[[Terraform Adoption Stages]]
