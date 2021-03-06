---
title: Azure AD Connect Health avec le rapport d’adresse IP à risque AD FS | Microsoft Docs
description: Décrit le rapport d’adresse IP à risque Azure AD Connect Health pour AD FS.
services: active-directory
documentationcenter: ''
ms.reviewer: zhiweiwangmsft
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/26/2019
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: 49b93cb7852692e4dad65fcbd72cd749db1b16fb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60350557"
---
# <a name="risky-ip-report-public-preview"></a>Rapport d’adresse IP à risque (version préliminaire publique)
Les clients AD FS peuvent exposer des points de terminaison d’authentification par mot de passe à Internet pour fournir des services d’authentification permettant aux utilisateurs finaux d’accéder aux applications SaaS telles qu’Office 365. Dans ce cas, il est possible pour un mauvais acteur de tenter de se connecter à votre système AD FS pour deviner le mot de passe d’un utilisateur final et accéder aux ressources de l’application. AD FS fournit la fonctionnalité de verrouillage de compte extranet pour éviter ce type d’attaques depuis AD FS dans Windows Server 2012 R2. Si vous utilisez une version antérieure, nous vous recommandons vivement de mettre à niveau votre système AD FS vers Windows Server 2016. <br />

En outre, il est possible qu’une seule adresse IP tente de se connecter plusieurs fois à la place de plusieurs utilisateurs. Dans ce cas, le nombre de tentatives par utilisateur peut se trouver sous le seuil pour la protection par verrouillage de compte dans AD FS. Azure AD Connect Health fournit désormais le « rapport d’adresse IP risquée » qui détecte cette condition et informe les administrateurs lorsque cela se produit. Voici les principaux avantages de ce rapport : 
- Détection des adresses IP qui dépassent un seuil d’échecs de connexion basée sur mot de passe
- Prise en charge des échecs de connexion dus à un mot de passe incorrect ou à un état de verrouillage extranet
- Notification par e-mail signalant la problème aux administrateurs dès qu’il survient avec paramètres d’e-mail personnalisables
- Paramètres de seuil personnalisables qui correspondent à la stratégie de sécurité d’une organisation
- Rapports téléchargeables pour analyse hors connexion et intégration avec d’autres systèmes via automatisation

> [!NOTE]
> Pour utiliser ce rapport, vous devez vous assurer que l’audit AD FS est activé. Pour plus d’informations, consultez [Activer l’audit pour AD FS](how-to-connect-health-agent-install.md#enable-auditing-for-ad-fs). <br />
> Pour accéder à l’aperçu, l’autorisation Administrateur général ou [Lecteur Sécurité](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#security-reader) est nécessaire.  
> 

## <a name="what-is-in-the-report"></a>Nouveautés dans le rapport
Les échecs de connexion dans des adresses IP client activité sont agrégés via les serveurs Proxy d’Application Web. Chaque élément du rapport d’adresse IP risquée affiche des informations agrégées sur les échecs de connexion AD FS qui dépassent le seuil défini. Il fournit les informations suivantes : ![Portail Azure AD Connect Health](./media/how-to-connect-health-adfs/report4a.png)

| Élément de rapport | Description |
| ------- | ----------- |
| Horodatage | Affiche l’horodatage basé sur l’heure locale du portail Azure au démarrage de la fenêtre de temps détection.<br /> Tous les événements quotidiens sont générés à minuit, heure UTC. <br />L’horodatage des événements se produisant toutes les heures est arrondi au début de l’heure. Vous pouvez trouver l’heure de début de la première activité sous l’élément « firstAuditTimestamp » du fichier exporté. |
| Type de déclencheur | Affiche le type de fenêtre de temps de détection. Les types de déclencheurs sont regroupés par heure ou par jour. Cela permet de faire la différence entre une attaque par force brute haute fréquence et une attaque lente, où le nombre de tentatives est distribué sur toute la journée. |
| Adresse IP | L’adresse IP risquée unique présentant une activité de mot de passe incorrect ou de connexion avec verrouillage extranet. Il peut s’agir d’une adresse IPv4 ou IPv6. |
| Nombre de mots de passe incorrects | Le nombre d’erreurs dues à un mot de passe incorrect sur l’adresse IP lors de la fenêtre de temps de détection. Les erreurs de mot de passe incorrect peuvent se produire plusieurs fois sur certains utilisateurs. Notez que cela n’inclut pas les tentatives ayant échoué en raison de mots de passe expirés. |
| Nombre d’erreurs de verrouillage extranet | Le nombre d’erreurs dues à un verrouillage extranet sur l’adresse IP lors de la fenêtre de temps de détection. Les erreurs de verrouillage extranet peuvent se produire plusieurs fois sur certains utilisateurs. Ces erreurs ne sont visibles que si le verrouillage extranet est configuré dans AD FS (versions 2012R2 ou une version ultérieure). <b>Remarque</b> Nous vous recommandons fortement d’activer cette fonction si vous autorisez les connexions extranet basées sur des mots de passe. |
| Tentative d’utilisateurs uniques | Le nombre de tentatives d’utilisateurs uniques sur l’adresse IP lors de la fenêtre de temps de détection. Cet élément permet de différencier une attaque d’utilisateur unique et une attaque de plusieurs utilisateurs.  |

Par exemple, l’élément de rapport ci-dessous indique que dans la fenêtre de 18h00 à 19h00, le 28/02/2018, l’adresse IP <i>104.2XX.2XX.9</i> n’affichait aucune erreur de mot de passe incorrect et présentait 284 erreurs de verrouillage extranet. 14 utilisateurs uniques ont été affectés. L’événement d’activité a dépassé le seuil par heure défini pour le rapport. 

![portail Azure AD Connect Health](./media/how-to-connect-health-adfs/report4b.png)

> [!NOTE]
> - Seules les activités dépassant le seuil désigné s’affichent dans la liste des rapports. 
> - Ce rapport peut contenir les activités des 30 derniers jours.
> - Ce rapport d’alerte n’affiche pas les adresses IP Exchange ou les adresses IP privées. Elles sont tout de même incluses dans la liste d’exportation. 
>

![portail Azure AD Connect Health](./media/how-to-connect-health-adfs/report4c.png)

## <a name="load-balancer-ip-addresses-in-the-list"></a>Charger des adresses IP d’équilibreur dans la liste
L’équilibreur de charge agrège les activités de connexion qui ont échoué et atteint le seuil d’alerte. Si vous voyez des adresses d’équilibreur de charge, il est très probable que votre équilibreur de charge externe n’envoie pas l’adresse IP cliente lorsqu’il transfère la requête au serveur proxy d’application web. Veuillez configurer correctement votre équilibreur de charge pour qu’il transfère l’adresse IP cliente. 

## <a name="download-risky-ip-report"></a>Télécharger le rapport d’adresse IP à risque 
Vous pouvez exporter la liste complète des adresses IP risquées des 30 derniers jours à l’aide de la fonctionnalité **Télécharger** du portail Connect Health Portal. Le résultat de l’exportation inclut tous les échecs de connexion à AD FS pour chaque fenêtre de temps de détection. Vous pouvez personnaliser les filtres après l’exportation. Outre les agrégations en surbrillance dans le portail, le résultat de l’exportation montre également plus de détails sur les activités de connexion ayant échoué par adresse IP :

|  Élément de rapport  |  Description  | 
| ------- | ----------- | 
| firstAuditTimestamp | Affiche l’horodatage des premiers échecs pendant la fenêtre de temps de détection.  | 
| lastAuditTimestamp | Affiche l’horodatage des derniers échecs pendant la fenêtre de temps de détection.  | 
| attemptCountThresholdIsExceeded | Un marquage indique si les activités en cours dépassent le seuil d’alerte.  | 
| isWhitelistedIpAddress | Un marquage indique si l’adresse IP est filtrée des alertes et des rapports. Les adresses IP privées (<i>10.x.x.x, 172.x.x.x et 192.168.x.x</i>) et les adresses IP Exchange sont filtrées et marquées comme True. Si vous voyez des plages d’adresses IP privées, il est très probable que votre équilibreur de charge externe n’envoie pas l’adresse IP client lorsqu’il transmet la requête au serveur proxy d’application web.  | 

## <a name="configure-notification-settings"></a>Configurer les paramètres de notification
Les administrateurs à contacter pour le rapport peuvent être mis à jour via les **Paramètres de notification**. Par défaut, la notification par e-mail des alertes d’adresse IP risquée est désactivée. Vous pouvez activer/désactiver la notification sous « Obtenir des notifications par e-mail pour les adresses IP dépassant le rapport de seuil d’échec d’activité ». Tout comme les paramètres de notification d’alerte génériques dans Connect Health, cela vous permet de personnaliser la liste des contacts à alerter sur les rapports d’adresse IP risquée. Vous pouvez également informer tous les administrateurs généraux lors de la modification de ce paramètre. 

## <a name="configure-threshold-settings"></a>Configurer les paramètres de seuil
Le seuil d’alerte peut être mis à jour dans les paramètres de seuil. Le seuil est initialement défini par défaut dans le système. Il existe quatre catégories dans les paramètres de seuil du rapport d’adresse IP risquée :

![portail Azure AD Connect Health](./media/how-to-connect-health-adfs/report4d.png)

| Élément de seuil | Description |
| --- | --- |
| (Verrouillage U/P + extranet incorrect) / Jour  | Paramètre de seuil permettant de signaler l’activité et de déclencher la notification d’alerte lorsque le nombre de mots de passe erronés et de verrouillages extranet dépasse ce seuil par **jour**. |
| (Verrouillage U/P + extranet incorrect) / Heure | Paramètre de seuil permettant de signaler l’activité et de déclencher la notification d’alerte lorsque le nombre de mots de passe erronés et de verrouillages extranet dépasse ce seuil par **heure**. |
| Verrouillage extranet / Jour | Paramètre de seuil permettant de signaler l’activité et de déclencher la notification d’alerte lorsque le nombre de verrouillages extranet dépasse ce seuil par **jour**. |
| Verrouillage extranet / Heure| Paramètre de seuil permettant de signaler l’activité et de déclencher la notification d’alerte lorsque le nombre de verrouillages extranet dépasse ce seuil par **heure**. |

> [!NOTE]
> - La modification du seuil du rapport est appliquée une heure après la modification de ce paramètre. 
> - Les éléments signalés existants ne seront pas affectés par la modification du seuil. 
> - Nous vous recommandons d’analyser le nombre d’événements détectés dans votre environnement et de régler le seuil en conséquence. 
>
>

## <a name="faq"></a>Forum Aux Questions
**Pourquoi est-ce que je reçois des plages d’adresses IP privées dans le rapport ?**  <br />
Les adresses IP privées (<i>10.x.x.x, 172.x.x.x et 192.168.x.x</i>) et les adresses IP Exchange sont filtrées et marquées comme True dans la liste verte d’adresses IP. Si vous voyez des plages d’adresses IP privées, il est très probable que votre équilibreur de charge externe n’envoie pas l’adresse IP client lorsqu’il transmet la requête au serveur proxy d’application web.

**Pourquoi vois-je adresses IP dans le rapport de l’équilibrage de charge ?**  <br />
Si vous voyez des adresses d’équilibreur de charge, il est très probable que votre équilibreur de charge externe n’envoie pas l’adresse IP cliente lorsqu’il transfère la requête au serveur proxy d’application web. Veuillez configurer correctement votre équilibreur de charge pour qu’il transfère l’adresse IP cliente. 

**Que faire pour bloquer l’adresse IP ?**  <br />
Vous devez ajouter l’adresse IP malveillante identifiée dans le pare-feu ou la bloquer dans Exchange.   <br />

**Pourquoi je ne vois pas tous les éléments de ce rapport ?** <br />
- Les activités de connexion ayant échoué ne dépassent pas les paramètres de seuil.
- Assurez-vous qu’aucune alerte vous avertissant que le service Health n’est pas à jour n’est active dans la liste des serveurs AD FS.  En savoir plus sur [la résolution de cette alerte](how-to-connect-health-data-freshness.md).
- Les audits ne sont pas activés dans les batteries de serveurs AD FS.

**Pourquoi n’est-ce que je reçois aucun accès au rapport ?**  <br />
L’autorisation Administrateur général ou [Lecteur Sécurité](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#security-reader) est nécessaire. Contactez votre administrateur global pour obtenir l’accès.


## <a name="next-steps"></a>Étapes suivantes
* [Azure AD Connect Health](whatis-hybrid-identity-health.md)
* [Installation de l’agent Azure AD Connect Health](how-to-connect-health-agent-install.md)
