---
title: Les plans de l’offre application SaaS Azure | Place de marché Azure
description: Créer un plan pour une offre d’application SaaS sur la place de marché Azure.
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 12/04/2018
ms.author: pabutler
ms.openlocfilehash: 2ff86b39f67b170ce99b045f5cfa888e06057bbe
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64943347"
---
# <a name="saas-application-plans-tab"></a>Onglet Plans d’application SaaS

Utilisez l’onglet Plans pour créer un plan. Au moins 1 plan doit être ajouté si vous utilisez l’option de vente via Microsoft pour votre application SaaS.

![Créer un plan](./media/saas-plans-new-plan.png)

## <a name="create-a-new-plan"></a>Créer un plan

Pour créer un plan :

1. Sous **Plans**, sélectionnez **+Nouveau plan**
2. Dans la fenêtre **Nouveau plan**, entrez un **ID de plan**. La longueur maximale est de 50 caractères. Cet ID doit contenir uniquement des caractères alphanumériques en minuscules, des tirets ou des traits de soulignement. Cet ID ne peut pas être modifié une fois l’offre publiée.
3. Cliquez sur **OK** pour enregistrer l’ID de l’offre.

   ![ID du plan du nouveau plan](./media/saas-plans-plan-ID.png)

### <a name="to-configure-the-plan"></a>Pour configurer le plan :

1. Sous **Détails du plan**, fournissez les informations dans les champs suivants :

   - Titre : indiquez le titre de ce plan. Le titre est limité à 50 caractères.
   - Description : ajoutez une description. La description est limitée à 500 caractères.
   - S’agit-il d’un plan privé ? Si le plan est accessible uniquement à un groupe choisi de clients, sélectionnez **Oui**.
   - Disponibilité par pays/région : le plan doit être disponible dans au moins 1 pays ou 1 région. Cliquez sur **Sélectionner des régions**. Sélectionnez un pays ou une région dans la liste **Sélectionner la disponibilité par pays/région**, puis sélectionnez **OK**. 
   - Tarification héritée : indiquez le coût en USD par mois.

2. Sous **Tarification simplifiée en devise**, fournissez les informations suivantes :

   - Conditions de facturation : la facturation mensuelle est sélectionnée par défaut. Vous pouvez également proposer une tarification annuelle.
   - Prix mensuel : indiquez le prix mensuel, qui doit correspondre à la tarification héritée.

   >[!NOTE] 
   >Si vous ajoutez une tarification annuelle, vous êtes invité à préciser le **tarif annuel** en USD par an.

3. Sélectionnez **Enregistrer** pour terminer la configuration du plan.

   >[!NOTE] 
   >Après avoir enregistré les modifications de la tarification, vous pouvez exporter/importer les données de tarification.

![Configurer les détails du plan](./media/saas-plans-plan-details.png)

## <a name="next-steps"></a>Étapes suivantes

[Onglet des informations de canal](./cpp-channel-info-tab.md)

