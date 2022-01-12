# Azure Function App with Azure Storage private endpoints

[![Deploy To Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fquickstarts%2Fmicrosoft.web%2Ffunction-app-vnet-integration%2Fazuredeploy.json)  [![Visualize](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/visualizebutton.svg?sanitize=true)](http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fquickstarts%2Fmicrosoft.web%2Ffunction-app-vnet-integration%2Fazuredeploy.json)

An Azure Function Premium plan with virtual network integration enabled allows the Azure Function to utilizes resources within the virtual network.

The following resources are deployed as part of the solution:

### Virtual Network

The virtual network into which the Azure Function Premium plan shall be integrated.

+ **Microsoft.Network/virtualNetworks**: The virtual network for which to integrate, and one subnet to which the function app plan is delegated.

### Elastic Premium Plan

The Azure Function app provisioned in this sample uses an [Azure Functions Elastic Premium plan](https://docs.microsoft.com/azure/azure-functions/functions-premium-plan#features). 

+ **Microsoft.Web/serverfarms**: The Azure Functions Premium plan (a.k.a. Elastic Premium plan)

### Azure Function App

The Function App uses the [AzureWebJobsStorage](https://docs.microsoft.com/azure/azure-functions/functions-app-settings#azurewebjobsstorage) and [WEBSITE_CONTENTAZUREFILECONNECTIONSTRING](https://docs.microsoft.com/azure/azure-functions/functions-app-settings#website_contentazurefileconnectionstring) app settings to connect to a private endpoint-secured Storage Account.

+ **Microsoft.Web/sites**: The function app instance.

### Azure Storage account

The Storage account that the Function uses for operation and for file contents. 

+ **Microsoft.Storage/storageAccounts**: [Azure Functions requires a storage account](https://docs.microsoft.com/azure/azure-functions/storage-considerations) for the function app instance.

### Application Insights

[Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) is used to provide [monitor the Azure Function](https://docs.microsoft.com/azure/azure-functions/functions-monitoring).

+ **Microsoft.Insights/components**: The Application Insights instance used by the Azure Function for monitoring.
