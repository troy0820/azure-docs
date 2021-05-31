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

## Before you begin

The article assumes [Azure CLI](https://github.com/Azure/azure-cli) is 2.24.0 or greater and that there is an existing [ARO](https://github.com/Azure/ARO-RP) cluster with the latest updates applied.

### Azure CLI Minimum Requirements

The minimum [Azure CLI](https://github.com/Azure/azure-cli) requirements to rotate ClusterServicePrincipal credentials within an [ARO](https://github.com/Azure/ARO-RP) cluster is 2.24.0.

To check the version of Azure CLI run:
```azurecli-interactive
# Azure CLI version
az -v

```
Minimum Azure CLI version:
```azurecli
# Azure CLI version
$ az -v
azure-cli                         2.24.0
```
To install or upgrade [Azure CLI](https://github.com/Azure/azure-cli) please follow these [steps](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).

## ClusterServicePrincipal Credential Rotation

ClusterServicePrincipal credential rotation has two methods:
 - Automated ClusterServicePrincipal Credential Rotation 
 - User Provided secret-id and client-secret ClusterServicePrincipal Credential Rotation.

### Automated ClusterServicePrincipal Credential Rotation

>[!NOTE]
> Will require user system administrator role to be executed

Automated ClusterServicePrincipal credential rotation will check if the ClusterServicePrincipal exists and rotate, if not it will create a new ClusterServicePrincipal.

Automatically rotate ClusterServicePrincipal credentials with the following command:

```azurecli-interactive
# Automatically rotate ClusterServicePrincipal credentials
az aro update --refresh-credentials
```

### User Provided secret-id and client-secret ClusterServicePrincipal Credential Rotation

>[!NOTE]
> Does not require user system administrator role to be executed

Manually rotate ClusterServicePrincipal credentials with user supplied secret-id and client-secret:

```azurecli-interactive
# Automatically rotate ClusterServicePrincipal credentials
az aro update --secret-id <secret-id> --client-secret <secret>
```

## Azure CLI ARO Update Help
For more details please see the Azure CLI ARO update help command:

```azurecli-interactive
# Azure CLI ARO Update Help
az aro update -h
```
