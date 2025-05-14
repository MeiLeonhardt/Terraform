# What is a tfvars file in Terraform?
source: https://spacelift.io/blog/terraform-tfvars
A tfvars file in Terraform is used to define input variables for a Terraform configuration. 
It’s a plain text file, typically written in HashiCorp Configuration Language (HCL) or JSON format.

**tfvars files are the best and most common due to their simplicity and effectiveness**
## Introduce variables
Imagine working in multiple environments, such as test, staging, and production. We would prefer to deploy a larger instance in production and a smaller instance in the test environment to save money. 
This means the code remains the same except for the instance type, which varies depending on the environment.
```
variable "vm_name" {
 type = string
 default = "Test"
 description = "VM name for testing"
}
```
Then, in the main.tf you can use your variable like ```var.vm_name``` in the right section of the block.

The above code works, but still relies on the default value. 
What if we want to change the instance type just for the production environment? How do we pass it a different value when deploying it to production?
```
💡 Pro tip
Whenever the number of variables increases significantly, it is wise to break down the project into various small projects.
```
## Create tfvars files
Let’s assign the variable a value using _terraform.tfvars_ file.

We will start by creating a file named _terraform.tfvars_ and assigning the ```instance_type``` variable a value in this file.

Great! We solved the problem of manually writing large commands and making edits to those commands. 
But the other half of the problem remains, the problem of managing different versions of the variable values for different environments.
- Should we manually edit the tfvars file every time we deploy to a different environment? 
- Would this solution scale for 10-20 variables with frequent deployments to various environments?

### How do we ask Terraform to use either of our versions then?

This can be done using the -var-file flag to specify the tfvars file to load. For instance, to load the production version, we will use the following command.

```terraform plan -var-file=”prod.tfvars”```
![image](https://github.com/user-attachments/assets/a213c206-bbc0-4767-9af5-ac648ddab119)

Awesome! We learned how to pass values to Terraform variables in a variety of ways and solved our many to many problems of managing multiple variables for many environments.
```
💡 **Pro tip: Should TFvars be committed?**
TFvars files should generally not be committed to version control, especially if they contain sensitive information such as credentials, API keys, or other secrets. These files are used to define variable values for Terraform configurations, and storing them in a repository can expose sensitive data.
If TFvars files must be included for non-sensitive default values, ensure that sensitive information is excluded and use .gitignore to prevent accidental commits of confidential data.
```
## terraform.tfvars vs variable.tf
Do not confuse _terraform.tfvars_ files with _variable.tf_ files. Both are completely different and serve different purposes.

_variable.tf_ are files where all variables are declared; these might or might not have a default value, while variable.tfvars are files where the variables are provided/assigned a value.
![image](https://github.com/user-attachments/assets/289ac4b7-bdcb-4259-8c3f-1456b1cc6104)
