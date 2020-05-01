## What are resource groups
- Logical grouping of resources
- life cycle. If you delete a resource group, all resources contained within are also deleted.
- Authorization. Resource groups are also a scope for applying role-based access control (RBAC) permissions. By applying RBAC permissions to a resource group, you can ease administration and limit access to allow only what is needed.


## Use resource groups for organization
- Consistent naming convention
- Organizing principles
  1. Organizing for authorization
  2. Organizing for life cycle
  3. Organizing for billing

## tags
A resource can have up to 50 tags. The name is limited to 512 characters for all types of resources except storage accounts, which have a limit of 128 characters. The tag value is limited to 256 characters for all types of resources. Tags aren't inherited from parent resources. Not all resource types support tags, and tags can't be applied to classic resources.

## Use policies to enforce standards
### What is Azure Policy?
Azure Policy is a service you can use to create, assign, and manage policies. These policies apply and enforce rules that your resources need to follow. These policies can enforce these rules when resources are created, and can be evaluated against existing resources to give visibility into compliance.
### Create a policy
### Create a policy assignment (enable the policy)
### Use policies to enforce standards

## Secure resources with role-based access control
Best Practices for RBAC

Here are some best practices you should use when setting up resources.

- Segregate duties within your team and grant only the amount of access to users that they need to perform their jobs. Instead of giving everybody unrestricted permissions in your Azure subscription or resources, allow only specific actions at a particular scope.
- When planning your access control strategy, grant users the lowest privilege level that they need to do their work.
- Use Resource Locks to ensure critical resources aren't modified or deleted (as you'll see in the next unit).

## Use resource locks to protect resources
### Create a resource lock
could be found in portal in resource group page in settings -> locks

## management group
![alt](img/)