# Individual Contributor  
- They have a very tight loop where what they do is write IaC in the form of Terraform config files. And the workflow is very simple, after writing the configuration you run a Terraform plan locally. You validate the plan and run an apply to create modify or destroy.
	- Then, much like an application, your definition of your infrastructure is a living, breathing thing, so today you deploy something and tomorrow you want to either extend it, scale it up or scale it down all by modifying your initial configuration files.  
 
# Single Team  
- When we move to a team setting we introduce a set of collaboration challenges. We are now modifying a single definition of what the infrastructure should look like but we have multiple people doing it. 
	- So how do we actually ensure that we have a consistent definition of our infrastructure and that we're making changes safely?  

-  The writing and planning remains the same but now instead of applying it ourselves we now modify in a commit to a version control system(Github, bitbucket, gitlab, etc.). The key here is that we have a single source of truth so even with multiple people editing the config there is only a single copy of what is being used to drive the configuration itself.
- We can now use that single source of truth to trigger Terraform applies from it.

# Multiple Teams  
- With a single team we really only have a single set of Terraform configuration that defined all of our infrastructure, but as we go to many teams this becomes impractical and there is too much coordination required and the configuration becomes complex.
- Instead we hierarchically decompose our infrastructure. 
	- We can have one team focus on the underlying network topology and cloud configuration,
	-  A series of middleware(logging, monitoring, security appliances), the underpinning shared infrastructure that our applications will consume.
	- At the edge we have our applications, these do not define any of the other components they are simply consumers. 
- On the whole we are composing these pieces to build our infrastructure but we are managing them in these smaller units called workspaces
	- On top of just separating it into different workspaces we can also use role-based access control. 
- It's really about how do we allow teams to work independently of each other but in a safe manner.
# Organizational Level  
- Once we reach this level there is a different set of governance challenges and it's less likely that the whole organization is Terraform enabled. 
- Here we often see a common pattern of publishers and consumers
	- What publishers do is push into a central registry modules that basically describe how to deploy different types of infrastructure(Java app, C# app, Database). You can think of these modules as an opinionated set of Terraform configuration.
	- Now a much larger group of consumers can pull from this registry without being intimately aware of Terraform or even the patterns. They can say "I have a java application, here is the JAR file, I want three of them and deploy this to AWS".
	- This gives them so levers and knobs to tweak but otherwise abstract away all the other complexity related to it. 
- The other challenge is how do we allow all this to be consumed by many users while still being safe.
	- Sentinel is an embedded policy as code framework which allows you to define a sandbox, and any operation  that is within that sandbox is allowed.
	- This will prevent things like deploying things to the wrong region, or bypassing security controls and rules. 
	- Without a policy as code system we would likely have to default to some form of ticketing system where changes are reviewed manually and we loose a lot of the efficiency compared to an agile self-service infrastructure. 