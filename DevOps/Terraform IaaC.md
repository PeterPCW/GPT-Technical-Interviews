# What is Infrastructure as Code (IaaC)? How does Terraform help to implement IaaC? Provide an example of a Terraform configuration file, and explain how it works.

Infrastructure as Code (IaaC) is the practice of managing infrastructure, including servers, networks, and storage, using code and automation tools. IaaC provides a way to define infrastructure as configuration files that can be versioned, reviewed, tested, and deployed using automation tools.

Terraform is an open-source tool for building, changing, and versioning infrastructure safely and efficiently. Terraform is designed to be cloud-agnostic, meaning it can manage resources across multiple cloud providers, as well as on-premises infrastructure.

A Terraform configuration file, also known as a Terraform module, is written in HashiCorp Configuration Language (HCL) and consists of a set of resources, variables, and providers. A resource is a declarative description of a specific infrastructure object, such as a virtual machine, network interface, or load balancer. A variable is a parameter that can be used to customize the behavior of a Terraform module, while a provider is a plugin that allows Terraform to manage resources in a specific cloud provider.

Here is an example of a Terraform configuration file that creates an Amazon Web Services (AWS) EC2 instance:

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

In this example, the **`provider`** block specifies that Terraform will use the AWS provider to manage resources in the us-east-1 region. The **`resource`** block describes an EC2 instance with the specified Amazon Machine Image (AMI) and instance type, and assigns it a tag with the name "example-instance".

When this Terraform configuration file is executed using the **`terraform apply`** command, Terraform will create the specified AWS EC2 instance, and store the current state of the infrastructure in a state file. Subsequent runs of the **`terraform apply`** command will compare the current state to the desired state described in the configuration file, and update the infrastructure as necessary to achieve the desired state.