---
title: Choisir une solution Azure pour le transfert périodique de données | Microsoft Docs
description: Découvrez comment choisir une solution Azure pour un transfert périodique de données.
services: storage
author: alkohli
ms.service: storage
ms.subservice: blobs
ms.topic: article
ms.date: 04/01/2019
ms.author: alkohli
ms.openlocfilehash: 8f106674c1b1ec90477c7c030dc55085fcf10656
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60729917"
---
# <a name="solutions-for-periodic-data-transfer"></a>Solutions de transfert périodique de données
 
Cet article offre une vue d’ensemble des solutions de transfert périodique de données. Le transfert périodique de données sur le réseau peut constituer un déplacement des données à intervalles réguliers ou continu. L’article décrit également les options de transfert de données recommandées et la matrice de fonctionnalités clés correspondant à ce scénario.

Pour une vue d’ensemble de toutes les options de transfert de données disponibles, voir [Choisir une solution de transfert de données Azure](storage-choose-data-transfer-solution.md).

## <a name="recommended-options"></a>Options recommandées

Les options recommandées pour le transfert périodique de données se divisent en deux catégories, selon que le transfert est récurrent ou continu.

- **Outils de script/programme** – pour un transfert de données à intervalles réguliers, utilisez des outils de script et de programme comme AzCopy et les API REST Stockage Azure. Ces outils ont pour cible les développeurs et les professionnels de l’informatique.

    - **AzCopy** – utilisez cet outil en ligne de commande pour copier facilement des données vers et à partir du Stockage Blob, Fichier et Table Azure avec des performances optimales. Il prend en charge la concurrence et le parallélisme, ainsi que la possibilité de reprendre les opérations de copie après une interruption.
    - **API/kits SDK Stockage Azure** – lorsque vous créez une application, vous pouvez la développer par rapport à l’API REST Stockage Azure et utiliser les kits SDK Azure proposés dans plusieurs langues. Les API REST peuvent également tirer parti de la bibliothèque de déplacement des données du Stockage Azure, conçue spécialement pour la copie de données hautes performances vers et à partir d’Azure.

- **Outils d’ingestion continue des données** – pour l’ingestion continue des données, vous pouvez sélectionner un des appareils de transfert en ligne Data Box ou Azure Data Factory. Ces outils, configurés par des professionnels de l’informatique, peuvent automatiser en toute transparence le transfert de données.

    - **Azure Data Factory** – Data Factory doit être utilisé pour monter en charge une opération de transfert, et si des fonctions d’orchestration et de monitoring de qualité professionnelle sont nécessaires. Avec Azure Data Factory, vous pouvez configurer un pipeline de cloud qui transfère régulièrement des fichiers entre plusieurs services Azure, en local ou les deux. Azure Data Factory permet d’orchestrer des workflows pilotés par les données qui ingèrent des données provenant de différents magasins de données et d’automatiser le déplacement et la transformation des données.
    - **Famille Azure Data Box pour les transferts en ligne** – Data Box Edge et Data Box Gateway sont des appareils réseau en ligne capables de déplacer des données vers et à partir d’Azure. Data Box Edge utilise un système de computing en périphérie compatible avec l’intelligence artificielle (IA) pour prétraiter les données avant le chargement. Data Box Gateway est une version virtuelle de l’appareil, offrant les mêmes fonctionnalités de transfert de données.


## <a name="comparison-of-key-capabilities"></a>Comparaison des principales fonctionnalités

Le tableau suivant résume les différences entre les principales fonctionnalités.

### <a name="scriptedprogrammatic-network-data-transfer"></a>Transfert de données réseau par script/programme

| Fonctionnalité                  | AzCopy                                 | API REST Stockage Azure       |
|-----------------------------|----------------------------------------|-------------------------------|
| Facteur de forme                 | Outil en ligne de commande de Microsoft       | Le développement se fait par rapport aux API <br> REST avec les bibliothèques clientes Azure |
| Installation ponctuelle initiale     | Minimales                                | Effort de développement modéré et variable    |
| Format de données                 | Blobs, Fichiers et Tables Azure | Blobs, Fichiers et Tables Azure   |
| Performances                 | Déjà optimisé                      | Optimisé au fil du développement                  |
| Tarifs                     | Gratuit, des frais de sortie de données s'appliquent      | Gratuit, des frais de sortie de données s’appliquent        |

### <a name="continuous-data-ingestion-over-network"></a>Ingestion continue des données sur le réseau

| Fonctionnalité                                       | Data Box Gateway | Data Box Edge   | Azure Data Factory        |
|----------------------------------|-----------------------------------------|--------------------------|---------------------------|
| Facteur de forme                                   | Appareil virtuel             | Appareil physique          | Service dans le Portail Azure, agent local                                                            |
| Matériel                                      | Votre hyperviseur            | Fourni par Microsoft    | N/D                                                            |
| Effort de configuration initial                          | Faible (< 30 minutes)            | Modéré (quelques heures) | Grand (plusieurs jours)                                                 |
| Format de données                                   | Blob et Fichiers Azure   | Blob et Fichiers Azure | [Prend en charge plus de 70 connecteurs de données pour les formats et les magasins de données](https://docs.microsoft.com/azure/data-factory/copy-activity-overview#supported-data-stores-and-formats)|
| Prétraitement des données                           | Non                          | Oui, avec le computing en périphérie    | Oui                                                           |
| Cache local<br>(pour stocker des données locales)    | Oui                        | Oui                      | Non                                                             |
| Transfert à partir d’autres clouds                    | Non                          | Non                        | Oui                                                           |
| Tarifs                                       | [Tarification](https://azure.microsoft.com/pricing/details/storage/databox/gateway/)                    | [Tarification](https://azure.microsoft.com/pricing/details/storage/databox/edge/)                  | [Tarification](https://azure.microsoft.com/pricing/details/data-factory/)                                                       |

## <a name="next-steps"></a>Étapes suivantes

- [Transférer des données avec AzCopy](/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json).
- [Plus d’informations sur le transfert de données avec l’API REST Stockage](https://docs.microsoft.com/azure/databox-online/data-box-gateway-deploy-add-shares).
- Comprendre comment :
    - [Transférer des données avec Data Box Gateway](https://docs.microsoft.com/azure/databox-online/data-box-gateway-deploy-add-shares).
    - [Transformer des données avec Data Box Edge avant de les envoyer à Azure](https://docs.microsoft.com/azure/databox-online/data-box-edge-deploy-configure-compute).
- [Découvrir comment transférer des données avec Azure Data Factory](https://docs.microsoft.com/azure/data-factory/tutorial-bulk-copy-portal).
