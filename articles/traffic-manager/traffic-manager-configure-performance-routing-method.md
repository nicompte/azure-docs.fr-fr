---
title: Configurer la méthode de routage du trafic selon les performances à l’aide d’Azure Traffic Manager | Microsoft Docs
description: Cet article explique comment configurer Traffic Manager pour router le trafic vers le point de terminaison présentant la latence plus faible.
services: traffic-manager
documentationcenter: ''
author: kumudd
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: kumud
ms.openlocfilehash: 4c948668e355b87026240588c6fac11d86e355b2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60883955"
---
# <a name="configure-the-performance-traffic-routing-method"></a>Configurer la méthode de routage en fonction des performances

La méthode de routage du trafic en fonction des performances vous permet de diriger le trafic vers le point de terminaison dont la latence est la plus faible dans le réseau du client. En règle générale, le centre de données dont la latence est la plus faible est géographiquement le plus proche. Cette méthode de routage du trafic ne peut pas prendre en compte des modifications en temps réel de la configuration ou de la charge du réseau.

##  <a name="to-configure-performance-routing-method"></a>Pour configurer la méthode de routage basé sur les performances

1. Dans un navigateur, connectez-vous au [portail Azure](https://portal.azure.com). Si vous ne possédez pas encore de compte, vous pouvez [vous inscrire pour bénéficier d’un essai gratuit d’un mois](https://azure.microsoft.com/free/). 
2. Dans la barre de recherche du portail, recherchez les **profils Traffic Manager** et cliquez sur le nom du profil pour lequel vous souhaitez configurer la méthode de routage.
3. Dans le panneau **Profil Traffic Manager**, vérifiez que les services cloud et les sites web que vous souhaitez inclure dans votre configuration sont présents.
4. Dans la section **Paramètres**, cliquez sur **Configuration** et dans le panneau **Configuration**, procédez comme suit :
    1. Pour les **paramètres de la méthode de routage du trafic**, pour la **méthode de routage** sélectionnez **Performances**.
    2. Définissez l’option **Paramètres de surveillance des points de terminaison** de manière identique pour tous les points de terminaison de ce profil comme suit :
        1. Sélectionnez le **protocole** approprié et spécifiez le numéro du **port**. 
        2. Pour **Chemin d’accès**, entrez une barre oblique */*. Pour surveiller les points de terminaison, vous devez indiquer un chemin et un nom de fichier. Une barre oblique (« / ») est une entrée valide pour le chemin d’accès relatif. Elle implique que le fichier se trouve dans le répertoire racine (par défaut).
        3. En haut de la page, cliquez sur **Enregistrer**.
5.  Testez les modifications dans votre configuration comme suit :
    1.  Dans la barre de recherche du portail, recherchez le nom du profil Traffic Manager et cliquez sur le profil Traffic Manager dans les résultats affichés.
    2.  Dans le panneau du profil **Traffic Manager**, cliquez sur **Vue d’ensemble**.
    3.  Le panneau **Profil Traffic Manager** affiche le nom DNS de votre profil Traffic Manager nouvellement créé. Celui-ci peut être utilisé par tous les clients (par exemple, en y accédant à l’aide d’un navigateur web) pour être routés vers le point de terminaison correct, comme déterminé par le type de routage. Dans ce cas, toutes les demandes sont routées vers le point de terminaison avec la latence la plus faible dans le réseau du client.
6. Une fois le profil Traffic Manager opérationnel, modifiez l’enregistrement DNS sur le serveur DNS faisant autorité, afin de faire pointer votre nom de domaine d’entreprise vers le nom de domaine Traffic Manager.

![Configuration de la méthode de routage du trafic selon les performances à l’aide de Traffic Manager][1]

## <a name="next-steps"></a>Étapes suivantes

- En savoir plus sur la [méthode de routage du trafic par pondération](traffic-manager-configure-weighted-routing-method.md).
- En savoir plus sur la [méthode de routage prioritaire](traffic-manager-configure-priority-routing-method.md).
- En savoir plus sur la [méthode de routage géographique](traffic-manager-configure-geographic-routing-method.md).
- Découvrez comment [tester les paramètres Traffic Manager](traffic-manager-testing-settings.md).

<!--Image references-->
[1]: ./media/traffic-manager-performance-routing-method/traffic-manager-performance-routing-method.png