---
title: Redémarrez la base de données Azure pour PostgreSQL - serveur unique à l’aide d’Azure CLI
description: Cet article décrit comment vous pouvez redémarrer une base de données Azure pour PostgreSQL - serveur unique à l’aide de l’interface CLI Azure
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 0a7cd815724fcebd6311860576e620eb9273523b
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65068973"
---
# <a name="restart-azure-database-for-postgresql---single-server-using-the-azure-cli"></a>Redémarrez la base de données Azure pour PostgreSQL - serveur unique à l’aide de l’interface CLI
Cette rubrique explique comment redémarrer un serveur Azure Database pour PostgreSQL. Vous pouvez avoir besoin de redémarrer votre serveur pour des raisons de maintenance, ce qui entraîne une brève interruption de service pendant que le serveur effectue l’opération.

Le redémarrage du serveur est bloqué si le service est occupé. Par exemple, le service peut traiter une opération précédemment demandée, telle que la mise à l’échelle de vCores.
 
Le temps nécessaire à un redémarrage varie selon le processus de récupération de PostgreSQL. Pour réduire le délai de redémarrage, nous vous recommandons de diminuer la quantité d’activités se produisant sur le serveur avant le redémarrage.

## <a name="prerequisites"></a>Conditions préalables
Pour utiliser ce guide pratique, il vous faut :
- Un [serveur Azure Database pour PostgreSQL](quickstart-create-server-up-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> Ce guide de procédures requiert l’utilisation de la version 2.0 Azure CLI ou version ultérieure. Pour vérifier la version, à l’invite de commande de l’interface Azure CLI, entrez `az --version`. Pour installer ou mettre à niveau Azure CLI, consultez [Installer Azure CLI]( /cli/azure/install-azure-cli).


## <a name="restart-the-server"></a>Redémarrez le serveur

Redémarrez le serveur avec la commande suivante :

```azurecli-interactive
az postgres server restart --name mydemoserver --resource-group myresourcegroup
```

## <a name="next-steps"></a>Étapes suivantes

En savoir plus sur [comment définir des paramètres dans la base de données Azure pour PostgreSQL](howto-configure-server-parameters-using-cli.md)