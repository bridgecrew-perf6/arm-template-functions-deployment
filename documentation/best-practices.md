# Best Practices Guide

### dependsOn  

Deployment sequence dependencies can be specified by using the dependsOn property. Dependencies are only allowed for resources that are deployed within the same template. 
For nested deployments, the dependency is created on the deployment resource itself. Dependencies are not needed for existing resources.

The value of a dependency is simply the name of a resource. A full `resourceId` can also be used for the dependency but is only necessary when two or more resources share the same name (which should be avoided).

Conditional resources are automatically removed from the dependency graph when not deployed. Authoring these dependencies can be done as if the resource will always be deployed.  

The following example shows declartion of dependsOn element, and for the full deployment template, see <a href="https://github.com/patelchandni/arm-template-functions-deployment/blob/main/templates/runfrompackage.json">Zip Deploy with Run From Package template</a>:

```json
        "dependsOn": [
                "[concat('Microsoft.Web/sites/', parameters('siteName'))]"
        ],
```  

While you may be inclined to use dependsOn to map relationships between your resources, it's important to understand why you're doing it. For example, to document how resources are interconnected, dependsOn isn't the right approach. You can't query which resources were defined in the dependsOn element after deployment. Setting unnecessary dependencies slows deployment time because Resource Manager can't deploy those resources in parallel.
