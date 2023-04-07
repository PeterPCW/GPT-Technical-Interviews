# Describe the Terraform execution plan and how it helps to manage infrastructure changes. How does Terraform determine what changes need to be made, and how does it execute those changes? Provide an example of how to generate an execution plan and apply it to a Terraform configuration.

In Terraform, an execution plan is a preview of the changes that Terraform will make to the infrastructure. It provides a detailed summary of the actions that Terraform will perform, including any new resources that will be created, existing resources that will be modified or destroyed, and any dependencies between resources.

To generate an execution plan, you can use the **`terraform plan`** command. This will analyze the current state of the infrastructure and the desired state defined in the Terraform configuration, and output a list of actions that Terraform will take to reach the desired state.

Once you have reviewed the execution plan and are satisfied with the proposed changes, you can apply the changes using the **`terraform apply`** command. Terraform will then create or modify the necessary resources to bring the infrastructure to the desired state.

Terraform determines what changes need to be made by comparing the current state of the infrastructure with the desired state defined in the Terraform configuration. It uses a state file to keep track of the current state of the infrastructure and uses a provider plugin to interact with the cloud provider API.

Terraform can manage a wide range of resources, including virtual machines, databases, load balancers, and DNS records. Each resource is defined using a resource block in the Terraform configuration, which specifies the type of resource, its attributes, and any dependencies.

Here's an example of how to create a virtual machine in AWS using Terraform:

```json
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"

  tags = {
    Name = "example-instance"
  }
}
```

In this example, the **`aws_instance`** resource creates a new EC2 instance in the **`us-east-1`** region with the **`t2.micro`** instance type and the specified AMI. The **`tags`** block sets a tag on the instance with a key of **`Name`** and a value of **`example-instance`**. To apply this configuration, you would run **`terraform apply`** after initializing the AWS provider.