# Best Practices Guide

### dependsOn  

Deployment sequence dependencies can be specified by using the dependsOn property. Dependencies are only allowed for resources that are deployed within the same template. 
For nested deployments, the dependency is created on the deployment resource itself. Dependencies are not needed for existing resources.

The value of a dependency is simply the name of a resource or copy loop. A full `resourceId` can also be used for the dependency but is only necessary when two or more resources share the same name (which should be avoided).

Conditional resources are automatically removed from the dependency graph when not deployed. Authoring these dependencies can be done as if the resource will always be deployed.  

Example:

```json
        "dependsOn": [
                "[concat('Microsoft.Web/sites/', parameters('siteName'))]"
        ],
```  


