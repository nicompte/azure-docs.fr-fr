---
title: Solution de VMware à CloudSimple - vue d’ensemble de machines virtuelles Azure
description: En savoir plus sur les machines virtuelles CloudSimple ainsi que leurs avantages.
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 77a2901ae2d81f42780110b1576c1f32c7ff397b
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65209487"
---
# <a name="cloudsimple-virtual-machines-overview"></a>Vue d’ensemble des machines virtuelles CloudSimple

CloudSimple vous permet de gérer des machines virtuelles VMware à partir du portail Azure.  Un cluster ou un pool de ressources à partir de votre cluster vSphere est géré via Azure en la mappant à votre abonnement.  Machine virtuelle de CloudSimple offre libre-service de gestion des machines virtuelles VMware à partir du portail Azure.  

Pour créer une VM CloudSimple à partir d’Azure, un modèle de machine virtuelle doit exister sur votre vCenter de Cloud privé.  Le modèle est utilisé pour personnaliser le système d’exploitation et des applications.  Le modèle de machine virtuelle peut être renforcé pour répondre aux stratégies de sécurité d’entreprise.  Vous pouvez utiliser le modèle pour créer des machines virtuelles et les utiliser à partir du portail Azure à l’aide d’un modèle en libre-service.

## <a name="benefits"></a>Avantages

Machines virtuelles de CloudSimple à partir du portail Azure fournissent un mécanisme de libre-service pour les utilisateurs à créer et gérer des machines virtuelles VMware.

* Créer une VM CloudSimple sur votre vCenter du Cloud privé
* Gérer les propriétés de la machine virtuelle
  * Ajouter/supprimer des disques
  * Ajouter/supprimer des cartes réseau
* Opérations d’alimentation de votre VM CloudSimple
  * Mise sous tension et hors tension
  * Réinitialiser la machine virtuelle
* Supprimer la machine virtuelle

## <a name="next-steps"></a>Étapes suivantes

* Découvrez comment [consommer des machines virtuelles VMware sur Azure](quickstart-create-vmware-virtual-machine.md)
* Découvrez comment [mapper votre abonnement Azure](https://docs.azure.cloudsimple.com/azure-subscription-mapping/)