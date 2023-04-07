# Explain how to use Terraform modules to manage reusable infrastructure components. What are some best practices for creating and using Terraform modules? Provide an example of a Terraform module that creates a load balancer in AWS.

Terraform modules are reusable components that can be used to create and manage infrastructure resources. A module can be thought of as a set of Terraform configuration files that can be used to create a specific set of resources with a common functionality or purpose.

Creating a module involves defining input variables, output variables, and resource blocks that define the infrastructure resources to be created. Input variables allow the user to customize the behavior of the module, while output variables provide a way to pass information back to the calling configuration. Resource blocks define the infrastructure resources that the module creates, such as instances, security groups, or load balancers.

Best practices for creating and using Terraform modules include:

- Keep modules small and focused on a single task or function.
- Use input variables to make modules more flexible and reusable.
- Define sensible defaults for input variables to minimize the need for customization.
- Use output variables to provide useful information about the resources created by the module.
- Document modules thoroughly, including examples of how to use them.
- Publish modules to a registry or repository to make them easily discoverable and shareable.

Here's an example of a Terraform module that creates a load balancer in AWS:

```json
// main.tf

variable "name" {
  type    = string
  default = "example"
}

variable "subnets" {
  type    = list(string)
}

variable "security_groups" {
  type    = list(string)
}

resource "aws_lb" "example" {
  name               = var.name
  internal           = false
  load_balancer_type = "application"
  subnets            = var.subnets
  security_groups    = var.security_groups

  tags = {
    Environment = "Production"
  }
}

output "load_balancer_dns_name" {
  value = aws_lb.example.dns_name
}
```

This module defines a variable for the load balancer name, as well as variables for the subnets and security groups to be used. It then creates an AWS load balancer resource using these variables, with the specified name and configuration. Finally, it defines an output variable that provides the DNS name of the load balancer for use in other configurations.

This module can be used in other configurations by calling it and passing in the necessary variables, like this:

```json
// example.tf

module "load_balancer" {
  source           = "modules/load_balancer"
  name             = "example-lb"
  subnets          = ["subnet-12345678", "subnet-87654321"]
  security_groups  = ["sg-12345678"]
}

output "load_balancer_dns_name" {
  value = module.load_balancer.load_balancer_dns_name
}
```

This configuration uses the load balancer module, passing in the required variables, and defines an output variable that provides the DNS name of the load balancer created by the module.