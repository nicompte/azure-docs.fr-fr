---
title: Fichier Include
description: Fichier Include
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/28/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 518c57bc3327511b70deef143826f2a1b9df8639
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61483596"
---
Avant de vous attribuez un rôle RBAC à un principal de sécurité, déterminez l’étendue d’accès que l’entité de sécurité doit avoir. Les méthodes conseillées préconisent qu’il est toujours préférable d’accorder la plus petite portée possible.

La liste suivante décrit les niveaux à laquelle vous pouvez étendre l’accès aux ressources d’objets blob et file d’attente Azure, en commençant par la plus petite portée :

- **Un conteneur spécifique.** Dans cette étendue, une attribution de rôle s’applique à tous les objets BLOB dans le conteneur, ainsi que les propriétés du conteneur et les métadonnées.
- **Une file d’attente individuel.** Dans cette étendue, une attribution de rôle s’applique aux messages dans la file d’attente, ainsi que les propriétés de la file d’attente et les métadonnées.
- **Le compte de stockage.** Dans cette étendue, une attribution de rôle s’applique à tous les conteneurs et leurs objets BLOB, ou pour toutes les files d’attente et leurs messages.
- **Le groupe de ressources.** Dans cette étendue, une attribution de rôle s’applique à tous les conteneurs ou les files d’attente dans tous les comptes de stockage dans le groupe de ressources.
- **L’abonnement.** Dans cette étendue, une attribution de rôle s’applique à tous les conteneurs ou les files d’attente dans tous les comptes de stockage dans tous les groupes de ressources dans l’abonnement.

> [!IMPORTANT]
> Si votre abonnement inclut un espace de noms Azure DataBricks, les rôles attribués à l’étendue de l’abonnement seront bloqués d’octroyer l’accès aux données blob et file d’attente.
