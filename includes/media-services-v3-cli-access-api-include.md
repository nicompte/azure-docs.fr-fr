---
title: Fichier Include
description: Fichier Include
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 05/01/2019
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: b0f93f950b55052ea8d8b31538c47226413dc82a
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65149187"
---
## <a name="access-the-media-services-api"></a>Accéder à l’API Media Services

Vous utilisez l’authentification de principal de service Azure AD pour vous connecter aux API Azure Media Services. La commande suivante crée une application Azure AD et attache un principal de service pour le compte. Vous devriez utiliser les valeurs renvoyées pour configurer votre application.

Avant d’exécuter le script, vous devez remplacer le `amsaccount` et `amsResourceGroup` par les noms que vous avez choisis lors de la création de ces ressources. `amsaccount` est le nom du compte Azure Media Services auquel attacher le principal de service.

La commande suivante renvoie une sortie `json` :

```azurecli
az ams account sp create --account-name amsaccount --resource-group amsResourceGroup
```

Cette commande génère une réponse semblable à celle-ci :

```json
{
  "AadClientId": "00000000-0000-0000-0000-000000000000",
  "AadEndpoint": "https://login.microsoftonline.com",
  "AadSecret": "00000000-0000-0000-0000-000000000000",
  "AadTenantId": "00000000-0000-0000-0000-000000000000",
  "AccountName": "amsaccount",
  "ArmAadAudience": "https://management.core.windows.net/",
  "ArmEndpoint": "https://management.azure.com/",
  "Region": "West US 2",
  "ResourceGroup": "amsResourceGroup",
  "SubscriptionId": "00000000-0000-0000-0000-000000000000"
}
```

Si vous souhaitez obtenir un `xml` dans la réponse, utilisez la commande suivante :

```azurecli
az ams account sp create --account-name amsaccount --resource-group amsResourceGroup --xml
```
