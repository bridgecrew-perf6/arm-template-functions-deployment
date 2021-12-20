# Best Practices Guide

### ZipDeploy with ARM template

ZipDeploy is intended for xcopy or ftp style deployment. By default, It unzips the artifacts and lay them out exactly to d:\home\site\wwwroot. You can use any tooling (such as one coming with Windows) to zip your content.

It is recommended to set appSettings WEBSITE_RUN_FROM_PACKAGE=1, to allow Zip package deployed with ZipDeploy to mount as read-only virtual filesystem directly without deflating or extracting. The advantage is to allow atomic and reliable deployment (no more files being locked). 

The following example shows declartion of ZipDeploy extension in site resources along with the recommended WEBSITE_RUN_FROM_PACKAGE appSetting, and for the full deployment template, see <a href="https://github.com/patelchandni/arm-template-functions-deployment/blob/main/templates/run-from-package.json">Zip Deploy with Run From Package template</a>:

```json
{
  "name": "[parameters('siteName')]",
  "type": "Microsoft.Web/sites",
  "apiVersion": "2016-03-01",
  "location": "[parameters('location')]",
  "properties": {
    "siteConfig": {
      "appSettings": [
        {
          "name": "WEBSITE_RUN_FROM_PACKAGE",
          "value": "1"
        }
      ]
    }
  },
  "resources": [
    {
      "name": "ZipDeploy",
      "type": "Extensions",
      "apiVersion": "2016-03-01",
      "dependsOn": [
        "[concat('Microsoft.Web/sites/', parameters('siteName'))]"
      ],
      "properties": {
        "packageUri": "[parameters('packageUri')]"
      }
    }
  ]
}
```

Since it will mount as read-only, app runtime will not be able to create or modify files under d:\home\site\wwwroot. In addition, Azure Functions Portal will also prevent you from modifying the Function Apps.

### dependsOn  

Deployment sequence dependencies can be specified by using the dependsOn property. Dependencies are only allowed for resources that are deployed within the same template. 
For nested deployments, the dependency is created on the deployment resource itself. Dependencies are not needed for existing resources.

The value of a dependency is simply the name of a resource. A full `resourceId` can also be used for the dependency but is only necessary when two or more resources share the same name (which should be avoided).

Conditional resources are automatically removed from the dependency graph when not deployed. Authoring these dependencies can be done as if the resource will always be deployed.  

The following example shows declartion of dependsOn element, and for the full deployment template, see <a href="https://github.com/patelchandni/arm-template-functions-deployment/blob/main/templates/run-from-package.json">Zip Deploy with Run From Package template</a>:

```json
"dependsOn": [
  "[concat('Microsoft.Web/sites/', parameters('siteName'))]"
],
```  

While you may be inclined to use dependsOn to map relationships between your resources, it's important to understand why you're doing it. For example, to document how resources are interconnected, dependsOn isn't the right approach. You can't query which resources were defined in the dependsOn element after deployment. Setting unnecessary dependencies slows deployment time because Resource Manager can't deploy those resources in parallel.
