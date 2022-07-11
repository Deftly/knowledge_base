Software isn't done when the code is working on your computer, or when the tests pass, or when someone gives you a "ship it" on a code review. Software isn't done till you *deliver* it to the user.

*Software delivery* consists of all of the work you need to do to make the code available to a customer, such as running that code on production servers, making the code resilient to outages and traffic spikes, and protecting the code from attackers. 

In this chapter we'll dive into the following topics:
- The rise of DevOps
- What is infrastructure as code?
- The benefits of infrastructure as code
- How Terraform works
- How Terraform compares to other infrastructure as code tools

# The Rise of DevOps
In the not-so-distant past, building a software company meant you also needed to manage a lot of hardware. It made sense to have one team typically called Developers("Devs"), dedicated to writing the software and a separate team, typically called Operations("Ops"), dedicated to managing the hardware.

The typical Dev team would build an application and hand it over to the Ops team who had to figure out how to deploy and run the application. Much of this was done manually, whether it be physically hooking up hardware(racking servers, hooking up network cables, etc.) or installing the application and its dependencies. 

This works initially but as the company grows you quickly run into problems. Because releases are done manually, as the number of servers increases, releases become slow, painful and unpredictable. The Ops team occasionally makes mistakes so you end up with *snowflake servers*, wherein each one has a subtly different configuration from all the others, a problem known as configuration drift. As a result the number of bugs increases and downtime and outages become more frequent. 

Ops teams, tired of being called in at 3 a.m. after every release, reduce the release cadence to once per week. Then to once per month. Then once every six months. Weeks before the biannual release, teams begin trying to merge all of their projects together leading to a huge mess of merge conflicts and an unstable release branch.

Nowadays, instead of managing their own data centers, many companies have moved to the cloud. Instead of investing heavily in hardware, many Ops teams are spending all their time working on software, using tools like Chef, Puppet, Terraform, Ansible, and Docker. As a result the distinction between the Dev and Ops teams is blurring. It still might make sense to have separate teams but it's clear that Dev and Ops need to work more closely together. This is where the *DevOps movement* comes in.

DevOps is a set of processes, ideas and techniques and everyone has a slightly different definition for the term but for now we'll think of it as the following:

> The goal of DevOps is to make software delivery vastly more efficient

Instead of multi-day merge nightmares you integrate code continuously and always keep it in a deployable state. Instead of deploying once per month you can deploy code dozen of times per day, or even after every single commit. And instead of constant outages and downtime, you build resilient, self-healing systems and use monitoring and alerting to catch problems that can't be resolve automatically. 

There are four core values in the DevOps movement: culture, automation, measurement, and sharing. We'll focus on just one of those values - automation. 

The goal is to automate as much of the software delivery process as possible. That means that you manage your infrastructure not by clicking around a web page or manually executing shell commands but through code. This is a concept that is typically called *infrastructure as code*.

# What is Infrastructure as Code?
The idea behind infrastructure as code (IaC) is that you write and execute code to define, deploy, update, and destroy you infrastructure. This is an important shift in mindset in which you treat all aspects of operations as software - even those aspects that represent hardware. In fact, the key insight of DevOps is that you can manage almost *everything* in code, including servers, databases, networks, log files, application configuration, documentation, automated tests, deployment processes, and so on.

There are five broad categories of IaC tools:
- Ad hoc scripts
- Configuration management tools
- Server templating tools
- Orchestration tools
- Provisioning tools

## Ad Hoc Scripts
The most straightforward approach to automating anything is to write down an *ad hoc script*. You take whatever task you were doing manually and break it down into discrete steps, use your favorite scripting language(Bash, Ruby, [[Why Choose Python 3|Python]]) to define each of those steps in code, and execute that script on your server.

```Bash
# Update the apt-get cache
sudo apt-get update

# Install PHP and Apache
sudo apt-get intall -y php apache2

# Copy the code from the repository
sudo git clone https://github.com/brikis98/php-app.git /var/www/html/app

# Start Apache
sudo service apache2 start
```

Whereas tools that are purpose-built for IaC provide concise APIs for accomplishing complicated tasks, if you're using a general-purpose programming language you need to write completely custom code for every task. IaC tools also generally enforce a particular structure for your code while with a general-purpose programming language each developer will use their own style and do something different. This isn't a big deal with an eight-line script like the one above but it very quickly devolves into a mess on unmaintainable spaghetti code as complexity increases.

## Configuration Management Tools
Chef, Puppet, and Ansible are all *configuration management* tools, which means they are designed to install and manage software on existing servers. Below is an *Ansible Role* which configures the same Apache web server as the Bash script from earlier.

```yml
- name: Update teh apt-get cache
  apt:
    update_cache: yes

- name: Install PHP
  apt:
    name: php

- name: Install Apache
  apt:
    name: apache2
AMI
- name: Copy the code from the repository
  git: repo=https://github.com/brikis98/php-app.git dest=/var/www/html/app

- name: Start Apache
  service: name=apache2 state=started enabled=yes
```

The code looks similar to the Bash script but using Ansible offers a number of advantages:

Coding conventions:
- Ansible enforces a consistent predictable structure, including documentation, file layout, clearly named parameters, secret management, and so on.

Idempotence:
- Code that works correctly no matter how many times you run it is called *idempotent code*. To make the Bash script from earlier idempotent you'd need to add many lines of code, including lots of if-statements. Most Ansible functions, on the other hand, are idempotent by default. The Ansible role above will install Apache only if it isn't installed already and will try to start the Apache web server only if it isn't running already.

 Distribution:
 - Ad hoc scripts are designed to run on a single local machine. Ansible and other configuration management tools are designed specifically for managing large numbers of remote servers. 

![[Ansible config management(1).png]]

- To apply the Ansible role to five servers, you first create a file called *hosts* that contains the IP addresses of those servers:

```yml
[webservers]
11.11.11.11
11.11.11.12
11.11.11.13
11.11.11.14
11.11.11.15
```

- Next, you define the following *Ansible playbook*:

```yml
- hosts: webservers
  roles:
  - webserver
```

- Finally, you execute the playbook as follows:

```bash
ansible-playbook playbook.yml
```

This instructs Ansible to configure all fire servers in parallel. Alternatively, by setting a parameter called *serial* in a playbook, you can do a *rolling deployment* which updates the servers in batches. Duplicating any of this logic in an ad hoc script would take dozens if not hundreds of lines of code. 

## Server Templating Tools
An alternative to configuration management that has been growing in popularity are *server templating tools* such as Docker, Packer, and Vagrant. Instead of launching a bunch of servers and configuring them by running the same code on each one the idea behind server templating tools is to create an *image* of a server that captures a fully self-contained "snapshot" of the OS, the software, the files, and all other relevant details. You can then use some other IaC tool to install that image on all of your servers.

![[Server Templating 1.png]]

There are 2 broad categories of tools for working with images:

### Virtual Machines
A *virtual machine* (VM) emulates an entire computer system, including the hardware. You run a *hypervisor* such as VMWare, VirtualBox, or Parallels to virtualize the underlying CPU, memory, hard drive, and networking. The benefit is that any *VM image* that you run on top of the hypervisor can see only the virtualized hardware so it's fully isolated from the host machine and any other VM images, and it will run exactly the same way in all environments(your computer, a QA server, a production server). The drawback is that virtualizing all this hardware and running a totally separate OS for each VM incurs a lot of overhead in terms of CPU usage, memory usage, and startup time. You can define VM images as code using tools such as Packer and Vagrant.

### Containers
A *container* emulates the user space of an OS. You run a container engine such as Docker, Podman, or cri-o to create isolated processes, memory, mount points, and networking. The benefit of this is that any container you run on top of the container engine can see only its own user space, so it's isolated from the host machine and other containers and will run the same way in all environments. The drawback is that all of the containers running on a single server share that server's OS kernel and hardware so it's much more difficult to achieve the level of isolation and security you get with a VM. However because the kernel and hardware are shared your containers can boot up in milliseconds and have virtually no CPU or memory overhead. You can define container images as code using tools such as Docker or Podman.  

![[VMs vs Containers.png]]

Below is a Packer template that creates an *Amazon Machine Image* (AMI), which is a VM image that you can run on AWS:

```json
{
  "builders":[{
    "ami_name": "packer-example",
    "instance_type": "t2.micro",
    "region": "us-east-2",
    "type": "amazon-ebs",
    "source_ami": "ami-0c55b159cbfafe1f0"
    "ssh_username": "ubuntu"
  }],
  "provisioners": [{
    "type": "shell"
    "inline": [
      "sudo apt-get update",
      "sudo apt-get install -y php apache2",
      "sudo git clone https://github.com/brikis98/php-app /var/www/html/app"
    ],
    "environment_vars": [
      "DEBIAN_FRONTENT=noninteractive"
    ]
  }]
}
```

This Packer template configures the same Apache web server that we've seen previously with the only difference being that this Packer template does not start the Apache web server. This is because server templates are typically used to install software in images but it's only when you run the image - for example, by deploying it on a server - that you should actually run the software.

You can build an AMI from this template by running ***packer build <template_file_name>*** and after the build completes you can install that AMI on all of your AWS servers, configure each to run Apache when the server is booting and they will all run exactly the same way. 

Different server templating tools can have different purposes. Packer is typically used to create images that you run directly on your production servers, such as an AMI that you run in your production AWS account. Vagrant is typically used to create images that you run on your development computers, such as a VirtualBox image that you run on your laptop. Docker is typically used to create images of individual applications. Docker images can be run on production or development machines as long as some other tool has configured the computer with the Docker Engine. 

A common pattern is to use Packer to create an AMI that has the Docker Engine installed, deploy that AMI on a cluster of servers in your AWS account, and then deploy individual Docker containers across the cluster to run your applications.

Sever templating is a key component in the shift to *immutable infrastructure*. This idea is inspired by [[Functional Programming|functional programming]] and entails variables are immutable so after you've set a variable to a value you can never change that variable again. If you need to update something you create a new variable. Because variables never change it's a lot easier to reason about your code. The idea behind immutable infrastructure is similar: once you've deployed a server, you never make changes to it again. If you need to update something, such as deploy a new version of your code, you create a new image from your server template and you deploy it on a new server.

## Orchestration Tools
Server templating tools are great for creating VMs and containers but how do you actually manage them? In a real world environment you'll likely a way to do the following:
- Deploy VMs and containers, making efficient use of your hardware
- Roll out updates to an existing fleet of VMs and containers using strategies like rolling deployment, blue-green deployment, and canary deployment.
- Monitor the health of your VMs and containers and automatically replace unhealthy ones (auto healing)
- Scale the number of VMs and containers up or down in response to load (auto scaling)
- Distribute traffic across your VMs and containers (load balancing)
- Allow your VMs and containers to find and talk to one another over the network (service discovery)

Handling these tasks is the realm of *orchestration tools* such as Kubernetes. Kubernetes allows you to define how to manage your Docker containers as code. You first deploy a *Kubernetes cluster*, which is a group of servers that Kubernetes will manage and use to run your Docker containers. Once you have a working a cluster, you can define how to run your Docker container as in a YAML file.

```yaml
apiVersion: apps/v1

# Use a Deployment to deploy mulitple replics of your Docker
# container(s) and to declaratively roll out updates to them

kind: Deployment

# Metadata about this Deployment, including its name
metadata:
  name: example-app

# The specification that configures this Deployment
spec:
  # This tells the Deployment how to find your container(s)
  selector:
    matchLabels:
      app: example-app

  # This tells the Deployment to run three replicas of your
  # Docer container(s)
  replicas: 3

  # Specifies how to update the Deployment. Here, we 
  # configure a rolling update.
  strategy:
    rollingUpdate:
      maxSurge: 3
      maxUnavailable: 0
    type: RollingUpdate

  # This is the template for what container(s) to deploy
  template:

    # The metadata for these container(s), including labels
    metadata:
      labels:
        app: example-app

    # The specification for your container(s)
    spec:
      containers:

        # Run Apache listening on port 80
        - name: example-app
          image: httpd:2.4.39
          ports:
            - containerPort: 80
```

The above file instructs Kubernetes to create a *Deployment* which is a declarative way to define:
- One or more Docker containers to run together. This group of containers is called a *Pod*. The Pod defined in the preceding code contains a single Docker container that runs Apache.
- The settings for each Docker container in the Pod. The Pod in the preceding code configures Apache to listen on port 80.
- How many copies (aka *replicas*) of the Pod to run in your cluster. The preceding code configures 3 replicas and Kubernetes automatically figures out where in your cluster to deploy each Pod, using a scheduling algorithm to pick the optimal servers in terms of high availability, resources, performance, and so on. It also constantly monitors the cluster to ensure that there are always three replicas running, automatically replacing any Pods that crash or stop responding.

## Provisioning Tools
Whereas configuration management, server templating, and orchestration tools define the code that runs on each server, *provisioning tools* such as Terraform and CloudFormation are responsible for creating the servers themselves. In fact, you can use provisioning tools to not only create servers, but also databases, caches, load balancers, queues, monitoring, subnet configurations, firewall settings, routing rules, Secure Sockets Layer (SSL) certificates, and almost every other aspect of your infrastructure. 

The following code deploys a web server using Terraform:

```hcl
resource "aws_instance" "app" {
  instance_type     = "t2.micro"
  availability_zone = "us-east-2a"
  ami               = "ami-0c55b159cbfafe1f0"

  user_data = <<-EOF
              #!/bin/bash
              sudo service apache2 start
              EOF
}
```

Initially we'll focus on just two parameters from the above code:

#### ami
- This parameter specifies the ID of an AMI to deploy on the server. You could set this parameter to the ID of the AMI built from the Packer template in the previous section, which ash PHP, Apache, and the application source code.

#### user_data
- This is a Bash script that executes when the web server is booting. The preceding code uses this script to boot up Apache.

This example shows you provisioning and server templating working together which is a common pattern in immutable infrastructure. 

# The Benefits of Infrastructure as Code
When your infrastructure is defined as code you are able to use a wide variety of software engineering practices to dramatically improve your software delivery process, including the following:

### Self-service
- most teams that deploy code manually have a small number of sysadmins(often, just one) who are the only ones who know all the magic incantations to make the deployment work and are the only ones with access to production. This becomes a major bottleneck as the company grows. If your Infrastructure is defined as code, the entire deployment process can be automated, and the developers can kick off their own deployments whenever necessary.

### Speed and safety
- If the deployment process is automated the process of deployment will be consistent, fast, repeatable, safe, and not prone to manual errors.

### Documentation
- IaC acts as documentation, allowing everyone in the organization to look at human readable source files to understand how things work.

### Version control
- You can store your IaC source files in version control, which means that the entire history of your infrastructure is now captured in the commit log. This becomes a powerful tool for debugging issues.

### Validation
- If the state of your infrastructure is defined in code, for every single change you can perform a code review, run a suite of automated tests, and pass the code through static analysis tools - all practices that significantly reduce the chance of defects.

### Reuse
- You can package your infrastructure into reusable modules, so that instead of doing every deployment for every product in every environment from scratch you can build on top of know, documented, and tested pieces.

# How Terraform Works
