---
title: Bonnes pratiques en matière de configuration d’appareil pour Azure IoT Hub | Microsoft Docs
description: En savoir plus sur les bonnes pratiques pour la configuration des appareils IoT à grande échelle
author: chrisgre
ms.author: chrisgre
ms.date: 06/24/2018
ms.topic: conceptual
ms.service: iot-hub
services: iot-hub
ms.openlocfilehash: c97395981ea3af90c7b0c590cb049fccc7392304
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60734828"
---
# <a name="best-practices-for-device-configuration-within-an-iot-solution"></a>Bonnes pratiques en matière de configuration d’appareil dans une solution IoT

La gestion automatique des appareils dans Azure IoT Hub automatise une grande partie des tâches répétitives et complexes liées à la gestion de grandes flottes d’appareils pendant tout leur cycle de vie. Cet article présente un grand nombre des bonnes pratiques pour les différents rôles impliqués dans le développement et l’exploitation d’une solution IoT.

* **IoT fabricant/intégrateur de matériel :** Les fabricants de matériel IoT, intégrateurs qui assemblent le matériel à partir de différents fabricants ou de fournisseurs de matériel pour un déploiement IoT réalisé ou intégré par d’autres fournisseurs. Ils sont impliqués dans le développement et l’intégration de microprogrammes, de systèmes d’exploitation incorporés et de logiciels incorporés.

* **Développeur de solutions IoT :** Le développement d’une solution IoT est généralement effectué par un développeur de solutions. Ce développeur peut faire partie d’une équipe interne ou être un intégrateur système spécialisé dans cette activité. Le développeur de solutions IoT peut développer plusieurs composants de la solution IoT en partant de zéro, intégrer de nombreux composants standard ou open source, ou encore personnaliser un [accélérateur de solution IoT](/azure/iot-accelerators/).

* **Opérateur de solutions IoT :** Une fois la solution IoT est déployée, elle nécessite des opérations à long terme, de surveillance, de mises à niveau et la maintenance. Ces tâches peuvent être assurées par une équipe interne comprenant des spécialistes en technologies de l’information, des équipes d’exploitation et de maintenance du matériel et des spécialistes du domaine qui contrôlent le comportement de l’infrastructure IoT globale.

## <a name="understand-automatic-device-management-for-configuring-iot-devices-at-scale"></a>Comprendre la gestion automatique des appareils pour la configuration des appareils IoT à grande échelle

La gestion automatique des appareils inclut les nombreux avantages des [jumeaux d’appareil](iot-hub-devguide-device-twins.md) et des [jumeaux de module](iot-hub-devguide-module-twins.md) pour synchroniser les états souhaités et signalés entre le cloud et les appareils. [Configurations d’appareils automatique](iot-hub-auto-device-config.md) automatiquement mettre à jour de grands ensembles de représentations et résumer de progression et conformité. La procédure générale suivante décrit la façon dont la gestion automatique des appareils est développée et utilisée :

* Le **fabricant ou intégrateur de matériel IoT** implémente les fonctionnalités de gestion des appareils dans une application incorporée à l’aide de [jumeaux d’appareil](iot-hub-devguide-device-twins.md). Ces fonctionnalités peuvent inclure des mises à jour de microprogramme, l’installation et la mise à jour de logiciels ainsi que la gestion des paramètres.

* Le **développeur de solutions IoT** implémente la couche de gestion des opérations de gestion des appareils à l’aide de [jumeaux d’appareil](iot-hub-devguide-device-twins.md) et de [configurations automatiques des appareils](iot-hub-auto-device-config.md). La solution doit inclure la définition d’une interface d’opérateur pour effectuer les tâches de gestion des appareils.

* **L’opérateur de solutions IoT** utilise la solution IoT pour effectuer les tâches de gestion des appareils, en particulier pour regrouper les appareils, lancer des modifications de configuration telles que les mises à jour de microprogramme, surveiller la progression et résoudre les problèmes qui se posent.

## <a name="iot-hardware-manufacturerintegrator"></a>Fabricant/intégrateur de matériel IoT

Voici les bonnes pratiques pour les fabricants de matériel et les intégrateurs qui traitent du développement de logiciels incorporés :

* **Implémentez [jumeaux](iot-hub-devguide-device-twins.md):** Représentations d’appareil activer la synchronisation de configuration souhaitée à partir du cloud et pour les rapports de configuration actuelle et les propriétés de l’appareil. La meilleure façon d’implémenter des jumeaux d’appareil dans les applications incorporées consiste à utiliser les [SDK Azure IoT](https://github.com/Azure/azure-iot-sdks). Les jumeaux d’appareil sont mieux adaptés à la configuration, car ils :

    * Prennent en charge la communication bidirectionnelle
    * Autorisent à la fois les états d’appareils connectés et déconnectés
    * Suivent le principe de cohérence éventuelle
    * Peuvent entièrement être interrogés dans le cloud

* **Structure de la représentation d’appareil pour la gestion des appareils :** La représentation d’appareil doit être structurée telles que les propriétés de gestion de périphérique sont logiquement regroupées en sections. En procédant ainsi, les modifications de configuration sont isolées sans affecter les autres sections du jumeau. Par exemple, créez une section avec les propriétés souhaitées pour le microprogramme, une autre section pour les logiciels et une troisième pour les paramètres réseau. 

* **Attributs de périphérique de rapport qui sont utiles pour la gestion des appareils :** Attributs tels que le périphérique physique marque et modèle, microprogramme, système d’exploitation, numéro de série, et les autres identificateurs sont utiles pour les rapports et en tant que paramètres pour le ciblage des modifications de configuration.

* **Définir les principaux États de création de rapports d’état et la progression :** États de niveau supérieur doivent être énumérés afin qu’ils peuvent être signalées à l’opérateur. Par exemple, une mise à jour de microprogramme signale un état comme Actuel, Téléchargement, Application, En cours et Erreur. Définissez des champs supplémentaires pour plus d’informations sur chaque état.

## <a name="iot-solution-developer"></a>Développeur de solutions IoT

Voici les bonnes pratiques pour les développeurs de solutions IoT qui génèrent des systèmes basés sur Azure :

* **Implémentez [jumeaux](iot-hub-devguide-device-twins.md):** Représentations d’appareil activer la synchronisation de configuration souhaitée à partir du cloud et pour les rapports de configuration actuelle et les propriétés de l’appareil. La meilleure façon d’implémenter des jumeaux d’appareil dans les applications de solutions cloud consiste à utiliser les [kits de développement logiciel (SDK) Azure IoT](https://github.com/Azure/azure-iot-sdks). Les jumeaux d’appareil sont mieux adaptés à la configuration, car ils :

    * Prennent en charge la communication bidirectionnelle
    * Autorisent à la fois les états d’appareils connectés et déconnectés
    * Suivent le principe de cohérence éventuelle
    * Peuvent entièrement être interrogés dans le cloud

* **Organiser les appareils à l’aide de balises de jumeau d’appareil :** La solution doit autoriser l’opérateur définir des anneaux de qualité ou d’autres ensembles d’appareils en fonction de différentes stratégies de déploiement telles que contrôle de validité. L’organisation des appareils peut être implémentée dans votre solution à l’aide de balises de jumeau d’appareil et de [requêtes](iot-hub-devguide-query-language.md). Elle est nécessaire pour permettre des déploiements de configuration avec précision et en toute sécurité.

* **Implémentez [configurations d’appareils automatique](iot-hub-auto-device-config.md):** Déploiement des configurations d’appareils automatique et la configuration du moniteur change à de larges groupes d’appareils IoT par le biais de représentations d’appareil. Les configurations automatiques des appareils ciblent des ensembles de jumeaux d’appareil via la **condition cible**, qui est une requête sur les balises ou propriétés signalées du jumeau d’appareil. Le **contenu cible** est l’ensemble des propriétés voulues qui seront définies dans les jumeaux d’appareil ciblés. Le contenu cible doit s’aligner sur la structure du jumeau d’appareil définie par le fabricant/l’intégrateur de matériel IoT.

   Les **métriques** sont des requêtes sur les propriétés signalées du jumeau d’appareil et doivent également être alignées sur la structure du jumeau d’appareil définie par le fabricant/l’intégrateur de matériel IoT. Les configurations automatiques des appareils bénéficient également d’IoT Hub qui effectue des opérations de jumeau d’appareil à une fréquence qui ne dépasse jamais les [seuils de limitation](iot-hub-devguide-quotas-throttling.md) pour les lectures et mises à jour de jumeaux d’appareil.

* **Utilisez le [Service Device Provisioning](../iot-dps/how-to-manage-enrollments.md):** Les développeurs de solutions doivent utiliser le Service Device Provisioning pour affecter des balises de jumeau d’appareil à de nouveaux périphériques, tels qu’ils seront automatiquement configurés par **configurations d’appareils automatique** qui visent les représentations associées à cette balise. 

## <a name="iot-solution-operator"></a>Opérateur de solutions IoT

Voici les bonnes pratiques pour les opérateurs de solutions IoT qui utilisent une solution IoT reposant sur Azure :

* **Organiser les appareils pour la gestion :** La solution IoT doit définir ou autoriser pour la création de sonneries de qualité ou d’autres ensembles d’appareils en fonction de différentes stratégies de déploiement telles que contrôle de validité. Les ensembles d’appareils seront utilisés pour déployer les modifications de configuration et effectuer d’autres opérations de gestion des appareils à grande échelle.

* **Effectuer des modifications de configuration à l’aide d’un lancement graduel :**  Déploiement progressif est un processus global par lequel un opérateur déploie les modifications apportées à un ensemble étendu d’appareils IoT. L’objectif est d’apporter des modifications progressivement afin de réduire le risque d’étendre les modifications avec rupture à une plus grande échelle.  L’opérateur doit utiliser l’interface de la solution pour créer une [configuration automatique de l’appareil](iot-hub-auto-device-config.md), et la condition de ciblage doit cibler un ensemble initial d’appareils (par exemple, un groupe de contrôle de validité). L’opérateur doit ensuite valider la modification de configuration dans l’ensemble initial d’appareils.

   Une fois la validation terminée, l’opérateur met à jour la configuration automatique de l’appareil pour inclure un plus grand ensemble d’appareils. L’opérateur doit également définir une priorité pour la configuration qui soit supérieure à d’autres configurations actuellement ciblées sur ces appareils. Le déploiement peut être surveillé à l’aide des métriques signalées par la configuration automatique de l’appareil.

* **Effectuez des restaurations dans le cas d’erreurs ou de mauvaises configurations :**  Une configuration automatique des appareils qui provoque des erreurs ou des erreurs de configuration peut être restaurée en modifiant le **condition de ciblage** afin que les appareils ne répondent pas à la condition de ciblage. Vérifiez qu’une autre configuration automatique de l’appareil de priorité inférieure est toujours ciblée sur ces appareils. Vérifiez que la restauration a réussi en consultant les mesures : La configuration restaurée ne doit plus afficher l’état pour les appareils non ciblées, et les mesures de la configuration du deuxième doivent désormais inclure des nombres pour les appareils qui sont toujours ciblés.

## <a name="next-steps"></a>Étapes suivantes

* Découvrez l’implémentation des jumeaux d’appareil dans [Comprendre et utiliser les jumeaux d’appareil IoT Hub](iot-hub-devguide-device-twins.md).

* Suivez les étapes pour créer, mettre à jour ou supprimer une configuration automatique de l’appareil dans [Configurer et surveiller des appareils IoT à grande échelle](iot-hub-auto-device-config.md).

* Implémenter un modèle de mise à jour du microprogramme à l’aide de jumeaux d’appareil et les configurations d’appareils automatique dans [didacticiel : Implémenter un processus de mise à jour de microprogramme de périphérique](tutorial-firmware-update.md).
