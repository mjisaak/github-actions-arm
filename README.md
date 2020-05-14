# GitHub Action ARM deployment sample

## Prerequisites

- GitHub Account
- Microsoft Azure subscription
- Service Principal

### GitHub Account

GitHub provides hosting for software development version control using Git. This sample will deploy Azure resources using GitHub Actions thus a GitHub Account is required.

[Create a free GitHub account](https://github.com/join)

### Microsoft Azure Subscription

Microsoft Azure is a cloud computing service created by Microsoft. It provides you a set of cloud services to help your organization meet your business challenges.
You will need an Azure Subscription in order to run this github-action-arm-deployment sample. If you don't own a Subscription already, you can create a free account here:

[Create a free Azure account](https://azure.microsoft.com/en-us/free/)

### Service Principal

The GitHub Action workflow will authenticate against Azure using a **service principal**. You can create the service principal either using the **Azure Portal** (see [How to: Use the portal to create an Azure AD application and service principal that can access resources](https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal)) or by using the [az ad sp create-for-rbac](https://docs.microsoft.com/en-us/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) **Azure CLI** command:

```powershell
$name = '[yourappname]'
$scope = '/subscriptions/[id]/resourceGroups/[rg]/'

az ad sp create-for-rbac --name $name --scopes $scope --sdk-auth
```

**Note:** You should follow the principle of least privilege and restrict the scope to a single Azure resource or a resource group.

Add the JSON output of the above command as a **secret** (common used name is ```AZURE_CREDENTIALS```) in your GitHub repository. See [creating secrets](https://github.com/Azure/actions-workflow-samples/blob/master/assets/create-secrets-for-GitHub-workflows.md#creating-secrets)

## Cleanup

Be sure to delete the resources when you are done with the sample. The following script will delete the resource group and all its resources. It will also delete the service principal we have created beforehand.

```powershell
az group delete --name $rgName --yes --no-wait
az ad sp delete --id "http://$name"
```

---

![Status](https://github.com/mjisaak/github-actions-arm/workflows/deployToAzure/badge.svg)
