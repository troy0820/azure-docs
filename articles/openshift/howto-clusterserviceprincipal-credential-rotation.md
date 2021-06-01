---
title: ClusterServicePrincipal Credential Rotation 
description: Discover how to rotate ClusterServicePrincipal credentials in Azure Red Hat OpenShift (ARO).
author: swiencki@redhat.com
ms.author: 
ms.service: azure-redhat-openshift
ms.topic: article
ms.date: 05/31/2021
#Customer intent: As an operator or developer, I need to rotate ClusterServicePrincipal credentials
---
# ClusterServicePrincipal Credential Rotation for your Azure Red Hat OpenShift (ARO) Cluster

The article provides the necessary details to rotate ClusterServicePrincipal credentials in Azure Red Hat OpenShift clusters ([ARO](https://github.com/Azure/ARO-RP)).

## Before You Begin

The article assumes [Azure CLI](https://github.com/Azure/azure-cli) is 2.24.0 or greater and that there is an existing ARO cluster with the latest updates applied.

### Azure CLI Minimum Requirements

The minimum Azure CLI requirements to rotate ClusterServicePrincipal credentials within an ARO cluster is 2.24.0.

To check the version of Azure CLI run:
```azurecli-interactive
# Azure CLI version
az -v
```
Minimum Azure CLI version:
```bash
# Azure CLI version
$ az -v
azure-cli                         2.24.0
```
To install or upgrade [Azure CLI](https://github.com/Azure/azure-cli) please follow these [steps](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).

## ClusterServicePrincipal Credential Rotation
>[!IMPORTANT]
>  ClusterServicePrincipal credential rotation can take upwards of 2 hours depending on cluster state.

ClusterServicePrincipal credential rotation has two methods:
 - Automated ClusterServicePrincipal credential rotation 
 - User Provided secret-id and client-secret ClusterServicePrincipal credential rotation

### Automated ClusterServicePrincipal Credential Rotation

>[!NOTE]
> Will require user system administrator role to be executed

Automated ClusterServicePrincipal credential rotation will check if the ClusterServicePrincipal exists and rotate, if not it will create a new ClusterServicePrincipal.

Automatically rotate ClusterServicePrincipal credentials with the following command:

```azurecli-interactive
# Automatically rotate ClusterServicePrincipal credentials
az aro update --refresh-credentials --name MyManagedCluster --resource-group MyResourceGroup
```

### User Provided secret-id and client-secret ClusterServicePrincipal Credential Rotation

>[!NOTE]
> Does not require user system administrator role to be executed

Manually rotate ClusterServicePrincipal credentials with user supplied secret-id and client-secret:

```azurecli-interactive
# Automatically rotate ClusterServicePrincipal credentials
az aro update --secret-id MySecretId --client-secret MyClientSecret --name MyManagedCluster --resource-group MyResourceGroup
```

## Troubleshoot
### ClusterServicePrincipal Expiration Date

ClusterServicePrincipal credentials have a set expiration date of a year and should be rotated within that given timeframe.

If the expiration date has passed the following errors are possible:

```bash
Failed to refresh the Token for request to MyResourceGroup StatusCode=401
Original Error: Request failed. Status Code = '401'.
[with]
Response body: {"error":"invalid_client","error_description": The provided client secret keys are expired. 
[or]
Response body: {"error":"invalid_client","error_description": Invalid client secret is provided.
```

To check the expiration date of ClusterServicePrincipal credentials run the following:

```azurecli-interactive
# ClusterServicePrincipal expiry in ISO 8601 UTC format
SP_ID=$(az aro show --name MyManagedCluster --resource-group MyResourceGroup \
    --query servicePrincipalProfile.clientId -o tsv)
az ad sp credential list --id $SP_ID --query "[].endDate" -o tsv
```

### Azure CLI ARO Update Help
For more details please see the Azure CLI ARO update help command:

```azurecli-interactive
# Azure CLI ARO update help
az aro update -h
```
