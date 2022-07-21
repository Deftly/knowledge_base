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

  ingress {
    from_port   = 8080
    to_port     = 8080
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
}
```

This code creates a new resource called **aws_security_group** and specifies that this group allows incoming TCP requests on port 8080 from CIDR block 0.0.0.0/0. *CIDR blocks* are a concise way to specify IP address ranges. For example, a CIDR block of 10.0.0.0/24 represents all IP addresses between 10.0.0.0 and 10.0.0.255. The CIDR block 0.0.0.0/0 is an IP address range that includes all possible IP addresses, so this security group allows incoming requests on port 8080 from any IP.

In addition to creating the security group you need to tell you EC2 Instance to use it by passing the ID of the security group into the **vpc_security_group_ids** argument of the **aws_instance** resource. To do this we uses Terraform *expressions* 