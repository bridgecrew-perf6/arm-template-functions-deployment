# Deploy to Exiting Function App with ZipDeploy Run From Package

[![Deploy To Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fpatelchandni%2Farm-template-functions-deployment%2Fmain%2Ffunction-app-basic-resources%2Fazuredeploy.json)  [![Visualize](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/visualizebutton.svg?sanitize=true)](http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2Fpatelchandni%2Farm-template-functions-deployment%2Fmain%2Ffunction-app-basic-resources%2Fazuredeploy.json)

This sample Azure Resource Manager template deploys an Azure Function App that communicates with the Azure Storage account referenced by the [AzureWebJobsStorage](https://docs.microsoft.com/azure/azure-functions/functions-app-settings#azurewebjobsstorage) and [WEBSITE_CONTENTAZUREFILECONNECTIONSTRING](https://docs.microsoft.com/azure/azure-functions/functions-app-settings#website_contentazurefileconnectionstring) app settings.

![Function App with Basic Resources](/function-app-basic-resources/images/function-app-basic-resources.jpg) 

### ZipDeploy with ARM template

ZipDeploy is intended for xcopy or ftp style deployment. By default, It unzips the artifacts and lay them out exactly to d:\home\site\wwwroot. You can use any tooling (such as one coming with Windows) to zip your content.

It is recommended to set appSettings `WEBSITE_RUN_FROM_PACKAGE=1`, to allow Zip package deployed with ZipDeploy to mount as read-only virtual filesystem directly without deflating or extracting. The advantage is to allow atomic and reliable deployment (no more files being locked). 

The following example shows declartion of ZipDeploy extension in site resources along with the recommended WEBSITE_RUN_FROM_PACKAGE appSetting, and for the full deployment template, see <a href="https://github.com/patelchandni/arm-template-functions-deployment/blob/main/templates/run-from-package.json">Zip Deploy with Run From Package template</a>:

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


