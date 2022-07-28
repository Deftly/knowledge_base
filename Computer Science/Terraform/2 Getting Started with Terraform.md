This chapter will cover the basics of how to use Terraform. It's an easy tool to learn so in just a short time you'll go from running your first Terraform commands all the way to using Terraform to deploy a cluster of servers with a load balancer to distribute traffic across them. This infrastructure is a good starting point for running scalable, highly available web services. In subsequent chapters we'll develop this example even further.

==The following assumes the installation of Terraform and authentication to AWS has been completed, see online documentation for help with these tasks.==

# Deploy a Single Server
Terraform code is written in the *HashiCorp Configuration Language* (HCL) in files with the extension *.tf*. It is a declarative language, so your goal is to describe the infrastructure you want, and Terraform will figure out how to create it. 

The first step to using Terraform is typically to configure the provider(s) you want to use. Create an empty folder and put a file in it called **main.tf** that contains the following:

```hcl
provider "aws" {
	region = "us-west-1"
}
```

This tells Terraform that you are going to be using AWS as your provider and that you want to deploy your infrastructure into the ***us-west-1*** region. An *AWS region* is a separate geographic area, such as **us-west-1** (N. California), **us-west-2** (Oregon), **eu-west-1** (Ireland), and **ap-southeast-2** (Sydney). Within each region, there are multiple isolated data centers known as *Availability Zones* (AZs), such as **us-east-2a**, **us-east-2b**, and so on. 

For each type of provider there are many different kinds of *resources* that you can create, such as servers, databases, load balancers, etc. The general syntax for creating a resource is Terraform is as follows:

```hcl
resource "<PROVIDER>_<TYPE>" "<NAME>" {
  [CONFIG...]
}
```

Where **PROVIDER** is the name of a provider (e.g **aws**), **TYPE** is the type of resource to create in that provider (e.g. **instance**), **NAME** is an identifier you can use throughout the Terraform code to refer to this resource, and **CONFIG** consists of one or more *arguments* that are specific to that resource. The example below deploys a single (virtual) server in AWS, know as an *EC2 Instance*.

```hcl
resource "aws_instance" "example" {
  ami           = "ami-085284d24fe829cd0"
  instance_type = "t2.micro"
}
```

The **aws_instance** resource supports many different arguments, but for now, you only need to set the two required ones:

#### ami
- The Amazon Machine Image to run on the EC2 Instance. You can find free and paid AMIs in the AWS Marketplace or create your own using tools such as Packer. The preceding code example sets the **ami** parameter to the ID of an Ubuntu image which is free to use.

#### instance_type
- The type of EC2 Instance to run. Each type of EC2 Instance provides a different amount of CPU, memory, disk space, and networking capacity. The preceding example uses **t2.micro**, which has one virtual CPU, 1 GB of memory, and is part of the AWS free tier.

### Use The Docs!

> Terraform supports hundreds of providers, each of which supports many resources, and each resource has many arguments. There is no way to remember them all. When you're writing Terraform code, you should be regularly referring to the Terraform documentation to look up what resources are available and how to use each one. 

In a terminal, go into the folder where you created **main.tf** and run the **terraform init** command:

```bash
[user@host]$ terraform init

Initializing the backend...

Initializing provider plugins...
- Finding latest version of hashicorp/aws...
- Installing hashicorp/aws v4.22.0...
- Installed hashicorp/aws v4.22.0 (signed by HashiCorp)

Terraform has created a lock file .terraform.lock.hcl to record the provider
selections it made above. Include this file in your version control repository
so that Terraform can guarantee to make the same selections by default when
you run "terraform init" in the future.

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```

The **terraform** binary contains the basic functionality for Terraform, but it does not come with the code for any of the providers. So when you're first starting to use Terraform you need to run **terraform init** to tell Terraform to scan the code, figure out what providers you're using, and download the code for them. By default, the provider code will be download into a **.terraform** folder, which is Terraform's scratch directory(you'll likely want to add it to .gitignore). 

Now that you have the provider code downloaded, run the **terraform plan** command:

```bash
[user@host]$ terraform plan

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with
the following symbols:
  + create

Terraform will perform the following actions:

  # aws_instance.example will be created
  + resource "aws_instance" "example" {
      + ami                                  = "ami-085284d24fe829cd0"
      + arn                                  = (known after apply)
      + associate_public_ip_address          = (known after apply)
      + availability_zone                    = (known after apply)
      + cpu_core_count                       = (known after apply)
      + cpu_threads_per_core                 = (known after apply)
      + disable_api_stop                     = (known after apply)
      + disable_api_termination              = (known after apply)
      + ebs_optimized                        = (known after apply)
      + get_password_data                    = false
      + host_id                              = (known after apply)
      + id                                   = (known after apply)
      + instance_initiated_shutdown_behavior = (known after apply)
      + instance_state                       = (known after apply)
      + instance_type                        = "t2.micro"
      + ipv6_address_count                   = (known after apply)
      + ipv6_addresses                       = (known after apply)
      (...)
}

Plan: 1 to add, 0 to change, 0 to destroy.
```

The **plan** command lets you see what Terraform will do before actually making any changes. This is a great way to sanity check your code before unleashing it onto the world. In the output of the **plan** command anything with a plus sign (+) will be created, anything with a minus sign (-) will be deleted, and anything with a tilde sign (~) will be modified in place. 

To actually create  the Instance, run the **terraform apply** command: 

```bash
[user@host]$ terraform apply

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with
the following symbols:
  + create

Terraform will perform the following actions:

  # aws_instance.example will be created
  + resource "aws_instance" "example" {
      + ami                                  = "ami-085284d24fe829cd0"
      + arn                                  = (known after apply)
      + associate_public_ip_address          = (known after apply)
      + availability_zone                    = (known after apply)
      + cpu_core_count                       = (known after apply)
      + cpu_threads_per_core                 = (known after apply)
      + disable_api_stop                     = (known after apply)
      + disable_api_termination              = (known after apply)
      + ebs_optimized                        = (known after apply)
      + get_password_data                    = false
      + host_id                              = (known after apply)
      + id                                   = (known after apply)
      + instance_initiated_shutdown_behavior = (known after apply)
      + instance_state                       = (known after apply)
      + instance_type                        = "t2.micro"
      + ipv6_address_count                   = (known after apply)
      + ipv6_addresses                       = (known after apply)
      (...)
}

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: 
```

You'll notice that the **apply** command shows you the same output as **plan** and asks you to confirm whether you actually want to proceed with this plan. So, while **plan** is available as a separate command, it's mainly useful for quick sanity checks and during code reviews ([[8 How to Use Terraform as a Team|will be covered more in chapter 8]]), and most of the time you'll run **apply** directly and review the plan output it shows you.

```bash
Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_instance.example: Creating...
aws_instance.example: Still creating... [10s elapsed]
aws_instance.example: Still creating... [20s elapsed]
aws_instance.example: Creation complete after 22s [id=i-004dd70568f4d8a7d]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

Congrats, you've just deployed an EC2 Instance in your AWS account using Terraform! We can confirm this by looking at the EC2 console in our AWS account.
![[Assets/EC2 instance Terraform.png]]

Sure enough, the Instance is there. We can see thought that the EC2 Instance doesn't have a name. To add one, you can add **tags** to the **aws_instance** resource:

```hcl
resource "aws_instance" "example" {
  ami           = "ami-085284d24fe829cd0"
  instance_type = "t2.micro"

  tags = {
    Name = "terraform-example"
  }
}
```

Run **terraform apply** again to see what this would do:

```bash
[user@host]$ terraform apply

aws_instance.example: Refreshing state... [id=i-004dd70568f4d8a7d]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with
the following symbols:
  ~ update in-place

Terraform will perform the following actions:

  # aws_instance.example will be updated in-place
  ~ resource "aws_instance" "example" {
        id                                   = "i-004dd70568f4d8a7d"
      ~ tags                                 = {
          + "Name" = "terraform-example"
        }
      ~ tags_all                             = {
          + "Name" = "terraform-example"
        }
        # (29 unchanged attributes hidden)

        # (7 unchanged blocks hidden)
    }

Plan: 0 to add, 1 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: 
```

Terraform keeps track of all the resources it already created for this set of configuration files, so it knows your EC2 Instance already exists (notice Terraform says **Refreshing state...** when you run the **apply** command), and it can show you a diff between what's currently deployed and what's in your Terraform code. This is one of the advantages of using a declarative tool over a procedural one, the diff shows that Terraform wants to create a single tag called **Name**. After applying the changes the names field will now display terraform-example.

Now that we have some working Terraform code we may want to store it in version control. This allows you to share code with team members, track the history of all infrastructure changes, and use the commit log for debugging. 

```bash
git init
git add main.tf
git commit -m "Initial commit"
```

You should also create a **.gitignore** that tells Git to ignore certain types of files so you don't accidentally check them in:

```text
.terraform
*.tfstate
*.tfstate.backup
```

The preceding **.gitignore** file instructs Git to ignore the **.terraform** folder which Terraform uses as a temporary scratch directory, as well as **\*.tfstate** files, which Terraform uses to store state. You'll see in [[3 How to Manage Terraform State|chapter 3]] why state files shouldn't be checked in. You should commit the **.gitignore** file too:

```bash
git add .gitignore
git commit -m "Add a .gitignore file"
```

To share this code with your teammates, you'll want to create a shared Git repository that you can all access. Configure your local Git repository to use the the shared remote repository:

```bash
git remote add origin git@github.com:<YOUR_USERNAME>/<YOUR_REPO_NAME>.git
```

Now, whenever you want to share your commits with your teammates, you can *push* them to **origin**:

```bash
git push origin main
```

And whenever you want to see changes your teammates have made, you can *pull* them from **origin**:

```bash
git pull origin main
```


# Deploy a Single Web Server
Our next step is to run a web server on this instance.The goal is to deploy the simplest web architecture possible: a single web server that can respond

![[Deploy simple web server Terraform.png]]

In a real-world case you'd likely built the web server using a web framework Like Ruby on Rails or Django but for now we'll run a very simple web server that always returns the text "Hello, World".

```shell
#!/bin/bash
echo "Hello, World" > index.html
nohup busybox httpd -f -p 8080 
```

This is a Bash script that writes the text "Hello, World" into index.html and runs a tool called **busybox** (which is installed by default on Ubuntu) to fire up a web server on port 8080. The **busybox** command is wrapped with **nohup** and **&** so that the web server runs permanently in the background, whereas the bash script itself can exit.

> The reason the example uses port 8080 rather than the default HTTP port 80 is that listening on any port less than 1024 requires root privileges. This is a security risk because any attacker who manages to compromise your server would get root privileges too. 

How do we get the EC2 instance to run this script? Normally you would use a tool like Packer to create a custom AMI that has the web server installed on it. Since the web server in this example is a simple one-liner that uses **busybox**, you can use a plain Ubuntu AMI and run the "Hello, World" script as part of the EC2 Instance's *User Data* configuration. When you launch an EC2 Instance, you have the option of passing either a shell script or cloud-init directive to User Data, and the EC2 Instance will execute it during boot. You pass a shell script to User Data by setting the **user_data** argument in the your Terraform code as follows:

```hcl
resource "aws_instance" "example" {
  ami           = "ami-085284d24fe829cd0"
  instance_type = "t2.micro"

  user_data = <<-EOF
  #!/bin/bash
  echo "Hello, World" > index.html
  nohup busybox httpd -f -p 8080 &
  EOF

  tags = {
    Name = "terraform-example"
  }
}
```

The **<<-EOF** and **EOF** are Terraform's *heredoc* syntax, which allows you to create multiline strings without having to insert new line characters everywhere.

By default, AWS does not allow any incoming or outgoing traffic from and EC2 Instance. To allow the EC2 Instance to receive traffic on port 8080, you need to create a *security group*:

```hcl
resouce "aws_security_group" "instance" {
  name = "terraform-example-instance"

  ingress = [ {
    cidr_blocks = [ "0.0.0.0/0" ]
    description = "example vpc"
    from_port = 8080
    to_port = 8080
    protocol = "tcp"
    ipv6_cidr_blocks = [ "0.0.0.0/0" ]
    prefix_list_ids = []
    security_groups = []
    self = false
  } ]
}
```

This code creates a new resource called **aws_security_group** and specifies that this group allows incoming TCP requests on port 8080 from CIDR block 0.0.0.0/0. *CIDR blocks* are a concise way to specify IP address ranges. For example, a CIDR block of 10.0.0.0/24 represents all IP addresses between 10.0.0.0 and 10.0.0.255. The CIDR block 0.0.0.0/0 is an IP address range that includes all possible IP addresses, so this security group allows incoming requests on port 8080 from any IP.

In addition to creating the security group you need to tell you EC2 Instance to use it by passing the ID of the security group into the **vpc_security_group_ids** argument of the **aws_instance** resource. To do this we uses Terraform *expressions*. An expression in Terraform is anything that returns a value. The simplest type of expressions are *literals* such as strings (e.g. "ami-ami-085284d24fe829cd0") and numbers, but Terraform also supports many other types of expressions.

One particularly useful type of expression is a *reference* which allows you to access values from other parts of your code. To access the ID of the security group resource, you are going to use a *resource attribute reference* which uses the following syntax:

```hcl
<PROVIDER>_<TYPE>.<NAME>.<ATTRIBUTE>
```

Where **PROVIDER** is the name of the provider (e.g. **aws**), **TYPE** is the type of resource (e.g. **security_group**), **NAME** is the name of that resource (e.g., the security group is named "**instance**"), and **ATTRIBUTE** is either one of the arguments of that resource (e.g., **name**) or one of the attributes *exported* by the resource (you can find the list of available attributes in the documentation for each resource). The security group exports an attribute called **id**, so the expression to reference it will look like this:

```hcl
aws_security_group.instance.id
```

You can use this security group ID in the **vpc_security_group_ids** argument of the **aws_instance**:

```hcl
resource "aws_instance" "example" {
  ami                    = "ami-085284d24fe829cd0"
  instance_type          = "t2.micro"
  vpc_security_group_ids = [aws_security_group.instance.id]

  user_data = <<-EOF
			  #!/bin/bash
			  echo "Hello, World" > index.html
			  nohup busybox httpd -f -p 8080 &
			  EOF
			
  tags = {
    Name = "terraform-example"
  }
}
```

When you add a reference from one resource to another you create an *implicit dependency*. Terraform parses these dependencies, builds a dependency graph from them, and uses that to automatically determine in which order it should create resources. You can even get Terraform to show you the dependency graph by running the graph command.

If you run the **apply** command you'll see that Terraform wants to create a security group and replace the EC2 Instance with a new one that has the new user data:

```bash
[user@host]$ terraform apply

(...) 
Terraform will perform the following actions: 

# aws_instance.example must be replaced 
-/+ resource "aws_instance" "example" { 
		  ami                    = "ami-0c55b159cbfafe1f0" 
		~ availability_zone      = "us-east-2c" -> (known after apply) 
		~ instance_state         = "running" -> (known after apply) 
		  instance_type          = "t2.micro" 
		  (...) 
		+ user_data              = "c765373..." # forces replacement 
		~ volume_tags            = {} -> (known after apply) 
		~ vpc_security_group_ids = [ 
			- "sg-871fa9ec",
		  ] -> (known after apply) 
		  (...) } 
	

  # aws_security_group.instance will be created
  + resource "aws_security_group" "instance" {
      + arn                    = (known after apply)
      + description            = "Managed by Terraform"
      + egress                 = (known after apply)
      + id                     = (known after apply)
      + ingress                = [
          + {
              + cidr_blocks      = [
                  + "0.0.0.0/0",
                ]
              + description      = "example vpc"
              + from_port        = 8080
              + ipv6_cidr_blocks = [
                  + "0.0.0.0/0",
                ]
              + prefix_list_ids  = [
                  + "value",
                ]
              + protocol         = "tcp"
              + security_groups  = [
                  + "value",
                ]
              + self             = false
              + to_port          = 8080
            },
        ]
      + name                   = "terraform-example-instance"
      + name_prefix            = (known after apply)
      + owner_id               = (known after apply)
      + revoke_rules_on_delete = false
      + tags_all               = (known after apply)
      + vpc_id                 = (known after apply)
    }

Plan: 2 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: 
```

The -/+ in the **plan** output means "replace"; look for the text "forces replacement" in the plan output to figure out what is forcing Terraform to do a replacement. Many of the arguments on the **aws_instance** resource will force a replacement if changed, which means that the original EC2 Instance will be terminated and a completely new Instance will be created. 

This is an example of the immutable infrastructure paradigm. It's worth mentioning that while the web server is being replaced, any users of that web server would experience downtime, we'll see how to do a zero-downtime deployment with Terraform in [[5 Terraform Tips and Tricks - Loops, If-Statements, Deployment, and Gotchas|Chapter 5]]. 

Once the plan has been applied and our new instance is running we can find it's public IP address in the description panel and then use **curl** to make an HTTP request to the IP address at port 8080:

```bash
[user@host] curl http://<EC2_INSTANCE_PUBLIC_IP>:8080
Hello, World
```


## Network Security
A VPC is partitioned into one or more *subnets* each with its own IP addresses. The subnets in the Default VPC are all *public subnets*, which means they get IP addresses that are accessible from the public internet, which is why you are able to test your EC2 Instance from your home computer.

Running a server in a public subnet is fine for a quick experiment but in real-world usage it's a security risk. Hackers are *constantly* scanning IP addresses at random for weaknesses. If your servers are exposed publicly, all it takes is accidentally leaving a single port unprotected or running out-of-date code with a know vulnerability, and someone can break in.

Therefore, for production systems, you should deploy all your servers, and certainly all of your data stores, in *private subnets*, which have IP addresses that can be accessed only from within the VPC and not from the public internet. The only servers you should run in public subnets are a small number of reverse proxies and load balancers that you lock down as much as possible.


# Deploy a Configurable Web Server
You might have noticed that our code has the port 8080 duplicated in both the security group and user data configuration. This violates the *Don't repeat Yourself (DRY)* principle: every piece of knowledge must have a single, unambiguous, authoritative representation within a system. Having the port number in multiple places it's easy to update it in one place but forget to make the same changes elsewhere.

To make our code more configurable and DRY Terraform allows you to define *input variables*. Here's the syntax for declaring a variable:

```hcl
variable "name" {
  [CONFIG ...]
}
```

The body of the variable declaration can contain three parameters, all of them optional:

**description**
It's always a good idea to use this parameter to document how the variable is used. The description helps make your code more readable and also is show when running the **plan** or **apply** commands.

**default**
There are a number of ways to provide a value for the variable including passing it in at the command line (using the **-var** option), via a file (using the **-var -file** option), or via an environment variable (Terraform looks for environment variables of the name **TF_VAR_<variable_name>**). If no value is passed in the variable will fall back to this default, and if there is no default value Terraform will interactively prompt the user for one.

**type**
This allows you to enforce *type constraints* on the variables a user passes in. Terraform supports a number of type constraints, including **string**, **number**, **bool**, **list**, **map**, **set**, **object**, **tuple**, and **any**. If you don't specify a type Terraform assumes the type is **any**.

---

We can combine type constraints. For example, here's a list input variable that requires all items in the list to be numbers:

```hcl
variable "list_numeric_example" {
  description = "An example of a numeric list in Terraform"
  type = list(number)
  default = [1, 2, 3]
}
```

We can also create more complicated *structural types* using the **object** and **tuple** type constraints:

```hcl
variable "object_example" {
  description = "An example of a structural type in Terraform"
  type        = object([
    name    = string
    age     = number
    tags    = list(string)
    enabled = bool
  ])

  default = {
    name    = "value1"
    age     = 42
    tags    = ["a", "b", "c"]
    enabled = true
  }
}
```

For the web server example, all we need is a variable that stores the port number:

```hcl
variable "server_port" {
  description = "The port the server will use for HTTP requests"
  type        = number
  default     = 8080
}
```

To use the value from an input variable in your Terraform code, you can use a type of expression called a *variable reference* which has the following syntax: **var.<VARIABLE_NAME>**. Below is our updated code using variable references.

```hcl
resource "aws_instance" "example" {
  ami                    = "ami-085284d24fe829cd0"
  type                   = "t2.micro"
  vpc_security_group_ids = [aws_security_group.instance.id]

  user_data = <<-EOF
              #!/bin/bash
              echo "Hello, World" > index.html
              nohup busybox httpd -f -p ${var.server_port} &
              EOF
}

resource "aws_security_group" "instance" {
  name = terraform-example-instance

 ingress = [{
   cidr_blocks      = ["0.0.0.0/0"]
   description      = "example-vpc"
   from_port        = var.server_port
   to_port          = var.server_port
   protocol         = "tcp"
   ipv6_cidr_blocks = ["0.0.0.0/0"]
   prefix_list_ids  = []
   security_groups  = []
   self             = false
 }]
}
```

In addition to input variables, Terraform also allows you to define *output variables* by using the following syntax:

```hcl
output "<NAME>" {
  value = <VALUE>
  [CONFIG ...]
}
```

The **NAME** is the name of the output variable, the **VALUE** can be any Terraform expression that you would like to output. The **CONFIG** can contain two additional parameters, both optional:

**description**
It's always a good idea to use this parameter to document what type of data is contained in the output variable.

**sensitive**
Set this parameter to **true** to instruct Terraform not to log this output at the end of **terraform apply**. This is useful if the output variable contains sensitive material or secrets such as passwords or private keys.

--- 

For example, instead of having to manually poke around the EC2 console to find the IP address of your server, you can provide the IP address as an output variable:

```hcl
output "public_ip" {
  value       = [aws_instance.example.public_ip]
  description = "The public IP address of the web server"
}
```

Input and output variables are essential ingredients in creating configurable and reusable infrastructure as code which will be covered in further detail in [[4 How to Create Reusable Infrastructure with Terraform Modules|chapter 4]].

# Deploying a Cluster of Web Servers
Running a single server is a good start but a single server is a single point of failure. If that server crashes or becomes overloaded from too much traffic, users will be unable to access your site. The solution is to run a cluster of servers, routing around servers that go down, and adjusting the size of the cluster based on traffic. 

Managing a cluster manually is a lot of work, fortunately you let AWS take care of it for you by using an Auto Scaling Group (ASG). An ASG automatically handles many tasks, including launching a cluster of EC2 Instances, monitoring the health of each Instance, replacing failed Instances, and adjusting the size of the cluster in response to load.

![[Auto Scaling Group Terraform.png]]

The first step in creating an ASG is to create a *launch configuration*, which specifies how to configure each EC2 Instance in the ASG. the **aws_launch_configuration** resource uses almost exactly the same parameters as the **aws_instance** resource (two of the parameters have different names: **ami** is now **image_id** and **vpc_security_group_ids** is now **security_groups**), so you can cleanly replace the latter with  the former:

```hcl
resource "aws_launch_configuration" "example" {
  image_id        = "ami-085284d24fe829cd0"
  instance_type   = "t2.micro"
  security_groups = [aws_security_group.instance.id]

  user_data = <<-EOF
			  #!/bin/bash
			  echo "Hello, World" > index.html
			  nohup busybox httpd -f -p ${var.server_port} &
			  EOF
}
```

Now you can create the ASG itself using the **aws_autoscaling_group** resource:

```hcl
resource "aws_autoscaling_group" "example" {
  launch_configuration = aws_launch_configuration.example.name

  min_size = 2
  max_size = 10

  tag {
    key                 = "Name"
    value               = "terraform-asg-example"
    propagate_at_launch = true
  }
}
```

This ASG will run between 2 and 10 EC2 Instances (defaulting to 2 for initial launch), each tagged with the name **terraform-asg-example**. The ASG uses a reference to fill in the launch configuration name, this leads to a problem: launch configurations are immutable, so if you change any parameter of your launch configuration, Terraform will try to replace it. Normally when replacing resources Terraform deletes the old resource first and then creates its replacement. However, since the ASG has a reference to the old resource Terraform won't be able to delete it. 

To solve this you can use a *lifecycle* setting. Every Terraform resource supports several lifecycle settings that configure how that resource is created, update, and/or deleted. In this case we can set the **create_before_destroy** setting to **true** and Terraform will create the replacement resource first, including updating any references that were pointing at the old resource, and then delete the old resource.

```hcl
resource "aws_launch_configuration" "example" {
  image_id = "ami-085284d24fe829cd0"
  instance_type = "t2.micro"
  security_groups = [aws_security_group.instance.id]

  user_data = <<-EOF
			  #!/bin/bash
			  echo "Hello, World" > index.html
			  nohup busybox httpd -f -p ${var.server_port} &
			  EOF

  # Required when using a launch configuration with an auto scaling group.
  lifecycle {
    create_before_destroy = true
  }
}
```

Another parameter that we need to add to the ASG is **subnet_ids**. This parameter specifies to into which VPC subnets the EC2 Instances should be deployed. Instead of hard coding the list of subnets we can use *data sources* to get the list of subnets in your AWS account.

A data source represents a piece of read-only information that is fetched from the provider every time you run Terraform. Adding a data source is just a way to query the provider's API for data and to make that data available to the rest of your Terraform code. The syntax for a data source is very similar to the syntax of a resource:

```hcl
data "<PROVIDER>_<TYPE>" "<NAME>" {
  [CONFIG ...]
}
```

And here is how we can use the **aws_vpc** data source to look up the data for the Default VPC:

```hcl
data "aws_vpc" "default" {
  default = true
}
```

To get the data out of a resource you use the following attribute syntax:

```hcl
data.<PROVIDER>_<TYPE>.<NAME>.<ATTRIBUTE>
```

For example, to get the ID of the VPC from the **aws_vpc** data source, you would use the following:

```hcl
data.aws_vpc.default.id
```

We combine this with another data source, **aws_subnets** to look up the subnets within that VPC:

```hcl
data "aws_subnets" "default" {
  filter {
    name = "vpc-id"
    value = [data.aws_vpc.default.id]
  }
}
```

Finally you can pull the subnet IDs out of the **aws_subnet_ids** data source and tell your ASG to use those subnets via the **vpc_zone_identifier** argument:

```hcl
resource "aws_autoscaling_group" "example" {
  launch_configuration = aws_launch_configuration.example.name
  vpc_zone_identifier = data.aws_subnets.default.ids

  min_size = 2
  max_size = 10

  tag {
    key                 = "Name"
    value               = "terraform-asg-example"
    propagate_at_launch = true
  }
}
```


# Deploying a Load Balancer
At this point we can deploy our ASG but we have to be able to route traffic to our multiple servers. One way to do this is to deploy a *load balancer* to distribute traffic across our servers and give all our users the IP (actually, the DNS name) of the load balancer. Creating a load balancer that is highly available and scalable is a lot of work but we can let AWS take care of it by using Amazon's *Elastic Load Balancer* (ELB) service.

![[ELB Terraform.png]]

AWS offers three different types of load balancers:

**Application Load balancer (ALB)**
Best suited for load balancing of HTTP and HTTPS traffic. Operates at the application layer (Layer 7) of the OSI model. 

**Network Load Balancer (NLB)**
Best suited for load balancing TCP, UDP, and TLS traffic. Can scale up and down in response to load faster than the ALB (the NLB is designed to scale to tens of millions of requests per second). Operates at the transport layer (Layer 4) of the OSI model.

**Classic Load Balancer (CLB)**
This is the "legacy" load balancer that predates the ALB and NLB. It can handle HTTP, HTTPS, TCP, and TLS traffic, but with far fewer features than either the ALB or NLB. Operates at both the application layer (Layer 7) and the transport layer (Layer 4) of the OSI model.

---

Most applications should use either the ALB or NLB. Because our example is simple and is an HTTP app without any extreme performance requirements the ALB is going to be the best fit.

The ALB consists of several parts:

![[ALB Terraform.png]]

**Listener**
Listens on a specific port and protocol

**Listener rule**
Takes requests that come into a listener and sends those that match specific paths (e.g., /foo and /bar) or hostnames (e.g., foo.example.com and bar.example.com) to specific target groups.

**Target groups**
One or more servers that receive requests from the load balancer. The target group also performs health checks on these servers and only sends requests to healthy nodes.

--- 

The first step is to create the ALB itself using the **aws_alb** resource:

```hcl
resource "aws_alb" "example" {
  name = "terraform-asg-example"
  load_balancer_type = "application"
  subnets = data.aws_subnets.default.ids
}
```

Note that the **subnets** parameter configures the load balancer to use all the subnets in your Default VPC by using the **aws_subnet_ids** data source. AWS load balancers don't consist of a single server but multiple servers that can run in separate subnets. They also automatically scale the number of load balancer servers based on traffic and handle fail-over so you get scalability and high availability out of the box.

Next we define a listener for this ALB using the **aws_lb_listener** resource:

```hcl
resource "aws_lb_listener" "http" {
  load_balancer_arn = aws_lb.example.arn
  port = 80
  protocol = "HTTP"
  
  # By default, return a simple 404 page
  default_action {
    type = "fixed-response"

    fixed_response {
      content_type = "text/plain"
      message_body = "404: page not found"
      status_code = 404
    }
  }
}
```

This listener configures the ALB to listen on the default HTTP port, port 80, use HTTP as the protocol, and send a simple 404 page as the default response for requests that don't match any listener rules.

Note that, by default, all AWS resources, including ALBs, don't allow any incoming or outgoing traffic, so you need to create a new security group specifically for the ALB. This security group should allow incoming requests on port 80 so that you can access the load balancer over HTTP, and outgoing requests on all ports so that the load balancer can perform health checks:

```hcl
resource "aws_security_group" "alb" {
  name = "terraform-example-alb"

  # Allow inbound HTTP requests
  ingress [{
    cidr_blocks = ["0.0.0.0/0"]
    from_port = var.alb_server_port
    to_port = var.alb_server_port
    protocol = "tcp"
    ipv6_cidr_blocks = ["0.0.0.0/0"]
    prefix_list_ids = []
    security_groups = []
    self = false
  }]

  # Allow all outbound requests
  egress = [{
    cidr_blocks = ["0.0.0.0/0"]
    from_port = 0
    to_port = 0
    protocol = "-1"
    ipv6_cidr_blocks = ["0.0.0.0/0"]
    prefix_list_ids = []
    security_groups = []
    self = false
  }]
}
```

You'll need to tell the **aws_lb** resource to use this security group via the **security_groups** argument:

```hcl
resource "aws_lb" "example" {
  name = "terraform-example-alb"
  load_balancer_type = "application"
  subnets = data.aws_subnets.default.ids
  security_groups = [aws_security_group.alb.id]
}
```

Next we need to create a target group for your ASG using the **aws_lb_target_group** resource:

```hcl
resource "aws_lb_target_group" "asg" {
  name = "terraform-asg-example"
  port = var.server_port
  protocol = "HTTP"
  vpc_id = data.aws_vpc.default.id

  health_check {
    path = "/"
    protocol = "HTTP"
    matcher = "200"
    interval = 15
    timeout = 3
    healthy_threshold = 2
    unhealthy_threshold = 2
  }
}
```

This group will health check your Instances by periodically sending an HTTP request to each Instance and will consider the Instance "healthy" only if the Instance returns a response that matches the configured **matcher**. If an Instance fails to respond, perhaps because that Instance has gone down or is overloaded, it will be marked as "unhealthy" and the target group will automatically stop sending traffic to it to minimize disruption for your users.

To tell the target group which EC2 Instance to send requests to we can take advantage of integration between the ASG and the ALB. In the **aws_autoscaling_group** resource we can set the **target_group_arns** argument to point at our new target group:

```hcl
resource "aws_autoscaling_group" "example" {
  launch_configuration = aws_launch_configuration.example.name
  vpc_zone_identifier = data.aws_subnets.default.ids

  target_group_arns = [aws_lb_target_group.asg.arn]
  health_check_type = "ELB"

  min_size = 2
  max_size = 10

  tag {
    key = "Name"
    value = "terraform-asg-example"
    propagate_at_launch = true
  }
}
```

We should also update the **health_check_type** to **"ELB"**. The default **health_check_type** is **"EC2"**, which is a minimal health check that considers and Instance unhealthy only if the AWS hypervisor says the VM is completely down or unreachable. The **"ELB"** health check instructs the ASG to use the target group's health check to determine whether the Instance is healthy and to automatically replace Instances if the target group reports them as unhealthy. That way, Instances will be replaced not only if they are completely down but also if they've stopped serving requests because they ran out of memory or a critical process crashed.

Finally it's time to tie all these pieces together by creating listener rules using the **aws_lb_listener_rule** resource:

```hcl
resource "aws_lb_listener_rule" "asg" {
  listener_arn = aws_lb_listener.http.arn
  priority = 100

  condition {
    path_pattern {
      values = ["*"]
    }
  }

  action {
    type = "forward"
    target_group_arn = aws_target_group.asg.arn
  }
}
```

The preceding code adds a listener rule that sends requests that match any path to the target group that contains your ASG.

One last thing before we deploy is to output the DNS name of the ALB:

```hcl
output "alb_dns_name" {
  value = aws_lb.example.dns_name
  description = "The domain name of the load balancer"
}
```

Now when we run **terraform apply** it should set up our infrastructure and output the **alb_dns_name**:

```bash
Outputs:
alb_dns_name = terraform-asg-example-1842076341.us-west-1.elb.amazonaws.com
```

After a few minutes for the Instances to boot we can test it by doing the following:

```bash
[user@host] curl http://<alb_dns_name>
Hello, World
```

Each time we access the URL, it'll pick a different Instance to handle the request. To further test our cluster you can go to the Instances tab and terminate one of the Instances. You should still get 200 OK responses when sending requests because the ALB will automatically detect that the Instance is down and stop routing to it as well as launch a new one to replace it (self-healing).

# Conclusion
This section should give a basic grasp of how to use Terraform. We can see that a declarative language makes it easy to describe exactly the infrastructure you want to create. Variables, references, and dependencies allow you to remove duplication from your code and make it highly configurable.

However this is all just the very basics, in [[3 How to Manage Terraform State|Chapter 3]] we'll cover how Terraform keeps track of what infrastructure is already created and the profound impact that has on how you should structure your Terraform code. In [[4 How to Create Reusable Infrastructure with Terraform Modules|Chapter 4]] we'll see how to create reusable infrastructure with Terraform modules.

#### Continue Reading
[[1 Why Terraform]]
[[3 How to Manage Terraform State]]
[[Terraform Up & Running table of contents]]