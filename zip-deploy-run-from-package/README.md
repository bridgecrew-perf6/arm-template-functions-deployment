# Function App Deployment with ZipDeploy Run From Package

### ZipDeploy with ARM template

ZipDeploy is intended for xcopy or ftp style deployment. By default, It unzips the artifacts and lay them out exactly to d:\home\site\wwwroot. You can use any tooling (such as one coming with Windows) to zip your content.

It is recommended to set appSettings `WEBSITE_RUN_FROM_PACKAGE=1`, to allow Zip package deployed with ZipDeploy to mount as read-only virtual filesystem directly without deflating or extracting. The advantage is to allow atomic and reliable deployment (no more files being locked). 

If you have an existing Function App with appSettings `WEBSITE_RUN_FROM_PACKAGE=1` and want to perform deployment with ZipDeploy extension, then here is the template:

[![Deploy To Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fpatelchandni%2Farm-template-functions-deployment%2Fmaster%2Fzip-deploy-run-from-package%2Fazuredeploy.json)

The following example shows declartion of ZipDeploy extension in site resources along with the recommended WEBSITE_RUN_FROM_PACKAGE appSetting (Please include rest of the existing app settings in the template to preserve them):

```json
{
  "name": "[parameters('siteName')]",
  "type": "Microsoft.Web/sites",
  "apiVersion": "2019-08-01",
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
      "apiVersion": "2019-08-01",
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
