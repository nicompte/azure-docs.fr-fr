---
title: Refactoriser une application Contoso en la migrant vers Azure Web App et Azure SQL Database | Microsoft Docs
description: Découvrez comment Contoso réhéberge une application locale en la migrant vers Azure Web App et une base de données Azure SQL Server.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/11/2018
ms.author: raynew
ms.openlocfilehash: 271e18d370068e0445f183af0c694b19f0da22f2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60672586"
---
# <a name="contoso-migration-refactor-an-on-premises-app-to-an-azure-web-app-and-azure-sql-database"></a>Migration de Contoso : Refactoriser une application locale sur une application web Azure et une base de données Azure SQL

Cet article explique comment Contoso refactorise son application SmartHotel360 dans Azure. Ils migrent la machine virtuelle front-end de l’application vers une application Web Azure, et la base de données de l'application vers une base de données Azure SQL.

Ce document fait partie d’une série d’articles qui montrent comment la société fictive Contoso migre ses ressources locales vers le cloud Microsoft Azure. La série comprend des informations générales et des scénarios qui illustrent comment configurer une infrastructure de migration, évaluer des ressources locales pour la migration et exécuter différents types de migration. Les scénarios augmentent en complexité. Nous ajouterons des articles au fil du temps.

**Article** | **Détails** | **État**
--- | --- | ---
[Article 1 : Vue d’ensemble](contoso-migration-overview.md) | Fournit une vue d’ensemble de la stratégie de migration de Contoso, de la série d’articles et des exemples d’application que nous utilisons. | Disponible
[Article 2 : Déployer une infrastructure Azure](contoso-migration-infrastructure.md) | Décrit comment Contoso prépare ses infrastructures locales et Azure pour la migration. La même infrastructure est utilisée pour tous les articles de migration. | Disponible
[Article 3 : Évaluer les ressources locales](contoso-migration-assessment.md)  | Montre comment Contoso évalue une application à deux niveaux locale SmartHotel s’exécutant sur VMware. Contoso évalue les machines virtuelles de l’application avec le service [Azure Migrate](migrate-overview.md) et la base de données SQL Server de l’application avec [l’Assistant Migration de données Azure](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Disponible
[Article 4 : Réhéberger une application sur des machines virtuelles Azure et une instance SQL Managed Instance](contoso-migration-rehost-vm-sql-managed-instance.md) | Montre comment Contoso exécute une migration lift-and-shift vers Azure pour l’application SmartHotel. Elle migre la machine virtuelle frontale de l’application à l’aide d’[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), et la base de données de l’application vers une instance SQL Managed Instance à l’aide du [service Azure Database Migration](https://docs.microsoft.com/azure/dms/dms-overview). | Disponible
[Article 5 : Réhéberger une application sur des machines virtuelles Azure](contoso-migration-rehost-vm.md) | Montre comment Contoso migre les machines virtuelles de l’application SmartHotel en utilisant uniquement Site Recovery. | Disponible
[Article 6 : Réhéberger une application sur des machines virtuelles et un groupe de disponibilité AlwaysOn SQL Server](contoso-migration-rehost-vm-sql-ag.md) | Montre comment Contoso migre l’application SmartHotel. Elle utilise Site Recovery pour migrer la machine virtuelle de l’application, et Database Migration Service pour migrer la base de données de l’application vers un cluster SQL Server protégé par un groupe de disponibilité AlwaysOn. | Disponible
[Article 7 : Réhéberger une application Linux sur des machines virtuelles Azure](contoso-migration-rehost-linux-vm.md) | Montre comment Contoso effectue une migration lift-and-shift de l’application osTicket Linux sur des machines virtuelles Azure à l’aide de Site Recovery. | Disponible
[Article 8 : Réhéberger une application Linux sur des machines virtuelles Azure et Azure MySQL Server](contoso-migration-rehost-linux-vm-mysql.md) | Montre comment Contoso migre l’application osTicket Linux vers des machines virtuelles Azure à l’aide de Site Recovery, et migre la base de données de l’application vers une instance Azure MySQL Server à l’aide de MySQL Workbench. | Disponible
Article 9 : Refactoriser une application sur une application web Azure et une base de données Azure SQL | Montre comment Contoso migre l’application SmartHotel vers une application web Azure, et migre la base de données d’application vers une instance de serveur SQL Azure | Cet article
[Article 10 : Refactoriser une application Linux vers Azure Web Apps et Azure MySQL](contoso-migration-refactor-linux-app-service-mysql.md) | Montre comment Contoso migre l’application Linux osTicket vers Azure Web Apps dans plusieurs sites intégrés avec GitHub pour assurer une livraison continue. Elle migre la base de données d’application vers une instance Azure MySQL. | Disponible
[Article 11 : Refactoriser TFS sur Azure DevOps Services](contoso-migration-tfs-vsts.md) | Montre comment Contoso migre son déploiement TFS (Team Foundation Server) local vers Azure DevOps Services dans Azure. | Disponible
[Article 12 : Réarchitecturer une application dans des conteneurs Azure et Azure SQL Database](contoso-migration-rearchitect-container-sql.md) | Montre comment Contoso migre et réarchitecture son application SmartHotel vers Azure. Elle réarchitecture la couche web d’application en tant que conteneur Windows et la base de données d’application en une base de données Azure SQL Database. | Disponible
[Article 13 : Regénérer une application dans Azure](contoso-migration-rebuild.md) | Montre comment Contoso regénère son application SmartHotel à l’aide d’une série de fonctionnalités et services Azure, notamment App Services, Azure Kubernetes, Azure Functions, Cognitive Services et Cosmos DB. | Disponible
[Article 14 : Mettre à l’échelle une migration vers Azure](contoso-migration-scale.md) | Après des essais de différentes combinaisons de migration, Contoso se prépare à une migration complète vers Azure. | Disponible

Dans cet article, Contoso migre Windows à deux niveaux. Application .NET SmartHotel360 s’exécutant sur des machines virtuelles VMware vers Azure. Si vous souhaitez utiliser cette application, elle est disponible en open source et vous pouvez la télécharger à partir de [GitHub](https://github.com/Microsoft/SmartHotel360).

## <a name="business-drivers"></a>Axes stratégiques

L’équipe informatique a travaillé en étroite collaboration avec des partenaires commerciaux pour comprendre le résultat qu’ils souhaitent obtenir avec cette migration :

- **Répondre à la croissance de l’entreprise** : Contoso étant en croissance, son infrastructure et ses systèmes locaux sont soumis à une charge importante.
- **Augmenter l’efficacité** : Contoso doit supprimer les procédures inutiles et rationaliser les processus pour les développeurs et les utilisateurs.  L’entreprise a besoin d’une informatique rapide et doit éviter de perdre du temps ou d’argent en répondant plus rapidement aux exigences des clients.
- **Accroître l’agilité** :  le service informatique de Contoso doit être plus réactif face aux besoins de l’entreprise. Elle doit être en mesure de réagir plus rapidement que l’évolution du marché pour réussir dans une économie mondiale.  L’informatique ne doit pas devenir une entrave à l’activité.
- **Mise à l’échelle** : à mesure que l’entreprise croît, le service informatique de Contoso doit fournir des systèmes capables de croître au même rythme.
- **Coûts** : Contoso souhaite réduire les coûts de licence.

## <a name="migration-goals"></a>Objectifs de la migration

L’équipe cloud de Contoso a épinglé les objectifs de cette migration. Ces objectifs ont été utilisés pour déterminer la meilleure méthode de migration.

**Configuration requise** | **Détails**
--- | --- 
**Application** | L’application dans Azure restera aussi critique qu’aujourd'hui.<br/><br/> Elle doit offrir les mêmes performances qu’actuellement dans VMWare.<br/><br/> L’équipe ne souhaite pas investir dans l’application. Pour l’instant, les administrateurs veulent simplement déplacer l’application de façon sécurisée vers le cloud.<br/><br/> L’équipe veut cesser de prendre en charge Windows Server 2008 R2, sur lequel l’application s’exécute actuellement.<br/><br/> L’équipe veut aussi passer de SQL Server 2008 R2 à une plateforme de base de données PaaS moderne, de façon à réduire les tâches de gestion.<br/><br/> Contoso souhaite autant que possible tirer parti de son investissement dans les licences SQL Server et dans Software Assurance.<br/><br/> En outre, Contoso veut limiter les risques liés à un point de défaillance unique sur le niveau web.
**Limitations** | L’application consiste en une application ASP.NET et un service WCF (Windows Communication Foundation) s’exécutant sur la même machine virtuelle. Ils veulent fractionner cela en deux applications web utilisant Azure App Service. 
**Microsoft Azure** | Contoso veut déplacer l’application vers Azure, mais ne veut pas l’exécuter sur des machines virtuelles. Contoso veut tirer parti des services PaaS d’Azure pour les niveaux web et données. 
**DevOps** | Contoso veut passer à un modèle DevOps, en utilisant Azure DevOps pour les builds et les pipelines de mise en production.

## <a name="solution-design"></a>Conception de la solution

Après avoir défini précisément les objectifs et exigences, Contoso conçoit et examine une solution de déploiement, et identifie le processus de migration, notamment les services Azure à utiliser pour la migration.

### <a name="current-app"></a>Application actuelle

- L’application SmartHotel360 locale est hiérarchisée sur deux machines virtuelles (WEBVM et SQLVM).
- Les machines virtuelles sont situées sur un hôte VMware ESXi **contosohost1.contoso.com** (version 6.5).
- L’environnement VMware est géré par le serveur vCenter Server 6.5 (**vcenter.contoso.com**) s’exécutant sur une machine virtuelle.
- Contoso dispose d’un centre de données local (contoso-datacenter), avec un contrôleur de domaine local (**contosodc1**).
- Les machines virtuelles locales dans le centre de données Contoso seront désaffectées une fois la migration terminée.


### <a name="proposed-solution"></a>Solution proposée

- Pour la couche Base de données de l’application, Contoso a comparé Azure SQL Database à SQL Server en se basant sur [cet article](https://docs.microsoft.com/azure/sql-database/sql-database-features). Contoso a choisi d’utiliser Azure SQL Database pour plusieurs raisons :
    - Azure SQL Database est un service managé de base de données relationnelle. Il offre des performances prévisibles à plusieurs niveaux de service, et ne nécessite pratiquement aucune administration. Ses avantages sont une extensibilité dynamique sans aucun temps d’arrêt, une optimisation intelligente intégrée, ainsi qu’une scalabilité et une disponibilité globales.
    - Contoso peut tirer parti de l’Assistant Migration de données (DMA) facile d’utilisation pour évaluer et migrer la base de données locale vers SQL Azure.
    - Grâce à Software Assurance, Contoso peut échanger les licences existantes contre des réductions de tarif sur SQL Database, en utilisant Azure Hybrid Benefit pour SQL Server. Cela pourrait leur permettre de réaliser jusqu’à 30 % d’économies.
    - Azure SQL Database fournit un certain nombre de fonctionnalités de sécurité, notamment le chiffrement permanent, le masquage dynamique des données, la sécurité au niveau des lignes et la détection des menaces.
- Pour le niveau web de l’application, Contoso a décidé d’utiliser Azure App Service. Ce service PaaS permet de déployer l’application avec seulement quelques changements de configuration. Contoso utilisera Visual Studio pour apporter les modifications et déployer deux applications web. L’une pour le site web, et l’autre pour le service WCF.
- Pour satisfaire aux exigences d’un pipeline DevOps, Contoso a décidé d’utiliser Azure DevOps pour la gestion du code source avec des dépôts Git. Des builds et la mise en production automatisées seront utilisées pour générer le code et pour le déployer sur Azure Web Apps.
  
### <a name="solution-review"></a>Examen de la solution
Contoso évalue la conception proposée en dressant une liste des avantages et des inconvénients.

**Considération** | **Détails**
--- | ---
**Avantages** | Le code de l’application SmartHotel360 ne nécessite pas de modification avant sa migration vers Azure.<br/><br/> Contoso peut tirer parti de son investissement dans Software Assurance en utilisant Azure Hybrid Benefit tant pour SQL Server que pour Windows Server.<br/><br/> Après la migration, Windows Server 2008 R2 ne devra plus être pris en charge. [Plus d’informations](https://support.microsoft.com/lifecycle)<br/><br/> Contoso peut configurer la couche web de l’application avec plusieurs instances, pour qu’elle ne constitue plus un point de défaillance unique.<br/><br/> La base de données ne dépendra plus de SQL Server 2008 R2, qui est une version déjà ancienne.<br/><br/> SQL Database répond aux exigences techniques. Contoso a évalué la base de données locale avec l’Assistant Migration de données, et a établit qu’elle était compatible.<br/><br/> Azure SQL Database intègre une tolérance de panne que Contoso n’aura pas à configurer. Cela signifie que la couche Données n’est plus un point de basculement unique.
**Inconvénients** | Azure App Services ne prend en charge le déploiement que d’une seule application pour chaque application Web. Cela signifie que deux applications web doit être approvisionnées (l’une pour le site web, et l’autre pour le service WCF).<br/><br/> Si Contoso utilise l’Assistant Migration de données au lieu de Database Migration Service pour migrer la base de données, elle ne disposera pas d’une infrastructure prête pour la migration de bases de données à grande échelle. Contoso devra créer une autre région pour garantir le basculement en cas d’indisponibilité de la région principale.

## <a name="proposed-architecture"></a>Architecture proposée

![Architecture du scénario](media/contoso-migration-refactor-web-app-sql/architecture.png) 


### <a name="migration-process"></a>Processus de migration

1. Contoso provisionne une instance SQL Azure et y migre la base de données SmartHotel360.
2. Contoso provisionne et configure Web Apps et y déploie l’application SmartHotel360.

    ![Processus de migration](media/contoso-migration-refactor-web-app-sql/migration-process.png) 

### <a name="azure-services"></a>Services Azure

**Service** | **Description** | **Coût**
--- | --- | ---
[Assistant Migration de données Microsoft (DMA)](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | Contoso utilisera DMA pour évaluer et détecter les problèmes de compatibilité susceptibles d’affecter les fonctionnalités de sa base de données dans Azure. DMA évalue la parité des fonctionnalités entre SQL sources et cibles, et recommande des améliorations des performances et de la fiabilité. | Cet outil est téléchargeable gratuitement.
[Base de données SQL Azure](https://azure.microsoft.com/services/sql-database/) | Service de base de données cloud relationnelle entièrement managé et intelligent. | Coût en fonction des fonctionnalités, du débit et de la taille. [Plus d’informations](https://azure.microsoft.com/pricing/details/sql-database/managed/)
[Azure App Services - Web Apps](https://docs.microsoft.com/azure/app-service/overview) | Créez des applications cloud performantes en utilisant une plateforme entièrement gérée. | Coût en fonction de la taille, de l’emplacement et de la durée d’utilisation. [Plus d’informations](https://azure.microsoft.com/pricing/details/app-service/windows/)
[Azure DevOps](https://docs.microsoft.com/azure/azure-portal/tutorial-azureportal-devops) | Fournit un pipeline d’intégration et de déploiement continus (CI/CD) pour le développement d’applications. Le pipeline démarre avec un dépôt Git pour la gestion du code de l’application, un système de build pour la production de packages et d’autres artefacts de build, et un système Release Management pour le déploiement de modifications sur les environnements de production, de test et de développement. 

## <a name="prerequisites"></a>Conditions préalables

Voici ce dont Contoso a besoin pour exécuter ce scénario :

**Configuration requise** | **Détails**
--- | ---
**Abonnement Azure** | Contoso a créé des abonnements dans un article précédent. Si vous n’avez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/pricing/free-trial/).<br/><br/> Si vous créez un compte gratuit, vous êtes l’administrateur de votre abonnement et pouvez effectuer toutes les actions.<br/><br/> Si vous utilisez un abonnement existant et que vous n’êtes pas l’administrateur, vous devez collaborer avec l’administrateur pour qu’il vous donne les autorisations Propriétaire ou Contributeur.
**Infrastructure Azure** | [Découvrez comment](contoso-migration-infrastructure.md) Contoso configure une infrastructure Azure.


## <a name="scenario-steps"></a>Étapes du scénario

Voici comment Contoso exécutera la migration :

> [!div class="checklist"]
> * **Étape 1 : Approvisionnez une instance SQL Database dans Azure** : Contoso approvisionne une instance SQL dans Azure. Une fois le site web de l’application migré vers Azure, l’application web du service WCF pointe sur cette instance.
> * **Étape 2 : Migrer la base de données avec l’Assistant Migration de données** : Contoso migre la base de données de l’application avec l’Assistant Migration de données.
> * **Étape 3 : Provisionner des applications Web Apps** : Contoso provisionne les deux applications web.
> * **Étape 4 : Configurer Azure DevOps** : Contoso crée un projet Azure DevOps et importe le dépôt Git.
> * **Étape 5 : Configuration des chaînes de connexion** : Contoso configure les chaînes de connexion afin que l’application web du niveau web, l’application web du service WCF et l’instance SQL puissent communiquer.
> * **Étape 6 : Configurer les pipelines de build et de mise en production** : Pour terminer, Contoso configure des pipelines de build et de mise en production pour créer l’application et pour la déployer sur deux applications Azure Web Apps distinctes.


## <a name="step-1-provision-an-azure-sql-database"></a>Étape 1 : Provisionner une base de données Azure SQL

1. Les administrateurs Contoso choisissent de créer une base de données SQL Database dans Azure. 

    ![Provisionner SQL](media/contoso-migration-refactor-web-app-sql/provision-sql1.png)

2. Ils spécifient un nom de base de données correspondant à la base de données en cours d’exécution sur la machine virtuelle locale (**SmartHotel.Registration**). Ils placent la base de données dans le groupe de ressources ContosoRG. Il s’agit du groupe de ressources qu’ils utilisent pour les ressources de production dans Azure.

    ![Provisionner SQL](media/contoso-migration-refactor-web-app-sql/provision-sql2.png)

3. Ils configurent une nouvelle instance de SQL Server (**sql-smarthotel-eus2**) dans la région primaire.

    ![Provisionner SQL](media/contoso-migration-refactor-web-app-sql/provision-sql3.png)

4. Ils définissent le niveau tarifaire pour qu’il corresponde à leurs besoins en matière de serveur et de base de données. Et ils choisissent d’économiser de l’argent avec Azure Hybrid Benefit, car ils ont déjà une licence SQL Server.
5. Pour le dimensionnement, ils utilisent un achat basé sur v-Core, et définissent les limites de leurs exigences attendues.

    ![Provisionner SQL](media/contoso-migration-refactor-web-app-sql/provision-sql4.png)

6. Ils créent ensuite l’instance de base de données.

    ![Provisionner SQL](media/contoso-migration-refactor-web-app-sql/provision-sql5.png)

7. Une fois l’instance créée, ils ouvrent la base de données et notent les informations dont ils auront besoin quand ils utiliseront l’Assistant Migration de données pour la migration.

    ![Provisionner SQL](media/contoso-migration-refactor-web-app-sql/provision-sql6.png)


**Besoin de plus d’aide ?**

- [Obtenir de l’aide](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal) pour le provisionnement d’une base de données SQL.
- [En savoir plus](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-elastic-pools) sur les limites de ressources v-Core.


## <a name="step-2-migrate-the-database-with-dma"></a>Étape 2 : Migrer la base de données avec le DMA

Les administrateurs Contoso vont migrer la base de données SmartHotel360 avec DMA.

### <a name="install-dma"></a>Installer DMA

1. Télécharger l’outil à partir du [Centre de téléchargement Microsoft](https://www.microsoft.com/download/details.aspx?id=53595) sur la machine virtuelle locale SQL Server (**SQLVM**).
2. Exécuter le programme d’installation (DownloadMigrationAssistant.msi) sur la machine virtuelle.
3. Dans la page **Terminer**, sélectionner **Lancer l’Assistant Migration de données Microsoft** avant la fin de l’exécution de l’Assistant.

### <a name="migrate-the-database-with-dma"></a>Migrer la base de données avec le DMA

1. Dans DMA, ils créent un projet (**SmartHotelDB**), puis sélectionnent **Migration** 
2. Ils sélectionnent le type de serveur source **SQL Server** et la cible **Azure SQL Database**. 

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-1.png)

3. Dans les détails de la migration, ils ajoutent **SQLVM** comme serveur source, et la base de données **SmartHotel.Registration**. 

     ![DMA](media/contoso-migration-refactor-web-app-sql/dma-2.png)

4. Ils reçoivent un message d’erreur qui semble être associé à l’authentification. Toutefois, après examen, le problème résulte de la présence d’un point (.) dans le nom de la base de données. Pour résoudre ce problème, ils décident de provisionner une nouvelle base de données SQL en utilisant le nom **SmartHotel-Registration**. Quand ils réexécutent DMA, ils peuvent sélectionner **SmartHotel-Registration** et continuer avec l’Assistant.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-3.png)

5. Dans **Sélectionner des objets**, ils sélectionnent les tables de base de données et génèrent un script SQL.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-4.png)

6. Après que DMS a créé le script, ils cliquent sur **Déployer le schéma**.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-5.png)

7. DMA vérifie que le déploiement a réussi.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-6.png)

8. À présent, ils commencent la migration.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-7.png)

9. Une fois la migration terminée, les administrateurs Contoso peuvent vérifier que la base de données est en cours d’exécution sur l’instance SQL Azure.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-8.png)

10. Ils suppriment la base de données SQL supplémentaire **SmartHotel.Registration** dans le portail Azure.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-9.png)


## <a name="step-3-provision-web-apps"></a>Étape 3 : Approvisionner les Web Apps

La base de données étant migrée, les administrateurs Contoso peuvent maintenant provisionner les deux applications web.

1. Ils sélectionnent **Application web** dans le portail.

    ![Application web](media/contoso-migration-refactor-web-app-sql/web-app1.png)

2. Ils fournissent un nom pour l’application (**SHWEB-EUS2**), exécutent celle-ci sur Windows, puis la placent dans le groupe de ressources de production **ContosoRG**. Ils créent ensuite un service et un plan d’application.

    ![Application web](media/contoso-migration-refactor-web-app-sql/web-app2.png)

3. Une fois l’application web approvisionnée, ils répètent le processus pour créer une application web pour le service WCF (**SHWCF-EUS2**)

    ![Application web](media/contoso-migration-refactor-web-app-sql/web-app3.png)

4. Cela fait, ils accèdent à l’adresse des applications pour vérifier si celles-ci ont été correctement créées.


## <a name="step-4-set-up-azure-devops"></a>Étape 4 : Configurer Azure DevOps


Contoso doit générer l’infrastructure et les pipelines DevOps pour l’application.  Pour cela, les administrateurs Contoso créent un projet DevOps, importent le code, puis configurent les pipelines de build et de mise en production.

1. Dans le compte Azure DevOps de Contoso, ils créent un projet (**ContosoSmartHotelRefactor**) et sélectionnent **Git** pour la gestion de version.

   ![Nouveau projet](./media/contoso-migration-refactor-web-app-sql/vsts1.png)
2. Ils importent le dépôt Git qui détient actuellement leur code d’application. Il se trouve dans un [dépôt public](https://github.com/Microsoft/SmartHotel360-internal-booking-apps) que vous pouvez télécharger.

    ![Télécharger le code d’application](./media/contoso-migration-refactor-web-app-sql/vsts2.png)
    
3. Une fois le code importé, ils connectent Visual Studio au dépôt et clonent le code à l’aide de Team Explorer.

    ![Se connecter à un projet](./media/contoso-migration-refactor-web-app-sql/devops1.png)

4. Une fois le dépôt cloné sur l’ordinateur du développeur, ils ouvrent le fichier solution de l’application. L’application web et le service wcf ont chacun un projet distinct dans le fichier.

    ![Fichier solution](./media/contoso-migration-refactor-web-app-sql/vsts4.png)
    

## <a name="step-5-configure-connection-strings"></a>Étape 5 : Configuration des chaînes de connexion

Les administrateurs Contoso doivent faire en sorte que les applications web et la base de données puissent communiquer. Pour ce faire, ils configurent des chaînes de connexion dans le code et dans les applications web.

1. Dans l’application web pour le service WCF (**SHWCF-EUS2**) > **Paramètres** > **Paramètres de l'application**, ils ajoutent une nouvelle chaîne de connexion nommée **DefaultConnection**.
2. La chaîne de connexion est extraite de la base de données **SmartHotel-Registration**, et doit être mise à jour avec les informations d’identification correctes.

    ![Chaîne de connexion](media/contoso-migration-refactor-web-app-sql/string1.png)

3. Dans Visual Studio, ils ouvrent le projet **SmartHotel.Registration.wcf** à partir du fichier solution. La section **connectionStrings** du fichier web.config pour le service WCF SmartHotel.Registration.Wcf doit être mise à jour avec la chaîne de connexion.

     ![Chaîne de connexion](media/contoso-migration-refactor-web-app-sql/string2.png)

4. La section **client** du fichier web.config pour SmartHotel.Registration.Web doit être modifiée de façon à ce qu’elle pointe vers le nouvel emplacement du service WCF. Il s’agit de l’URL de l’application web WCF qui héberge le point de terminaison de service.

    ![Chaîne de connexion](media/contoso-migration-refactor-web-app-sql/strings3.png)

5. Une fois les modifications apportées au code, les administrateurs doivent valider les modifications. À l’aide de Team Explorer dans Visual Studio, ils valider et synchroniser.


## <a name="step-6-set-up-build-and-release-pipelines-in-azure-devops"></a>Étape 6 : Configurer les pipelines de build et de mise en production dans Azure DevOps

Les administrateurs de Contoso configurent maintenant Azure DevOps pour lancer le processus de build et mise en production.

1. Dans Azure DevOps, ils cliquent sur **Build et mise en production** > **Nouveau pipeline**.

    ![Nouveau pipeline](./media/contoso-migration-refactor-web-app-sql/pipeline1.png)

2. Ils sélectionnent **Azure Repos Git** et le dépôt pertinent.

    ![Git et dépôt](./media/contoso-migration-refactor-web-app-sql/pipeline2.png)

3. Dans **Sélectionner un modèle**, ils sélectionnent le modèle ASP.NET pour leur build.

     ![Modèle ASP.NET](./media/contoso-migration-refactor-web-app-sql/pipeline3.png)
    
4. Le nom **ContosoSmartHotelRefactor-ASP.NET-CI** est utilisé pour la build. Ils cliquent sur **Enregistrer et mettre en file d’attente**.

     ![Enregistrer et mettre en file d’attente](./media/contoso-migration-refactor-web-app-sql/pipeline4.png)

5. La première build est alors lancée. Ils cliquent sur le numéro de build pour surveiller le processus. Une fois le processus terminé, ils peuvent voir les commentaires associés à ce dernier, puis cliquer sur **Artefacts** pour passer en revue les résultats de la build.

    ![Révision](./media/contoso-migration-refactor-web-app-sql/pipeline5.png)

6. Le dossier **drop** contient les résultats de la build.

   - Les deux fichiers zip sont les packages qui contiennent les applications.
   - Ces fichiers sont utilisés dans le pipeline de mise en production pour le déploiement sur Azure Web Apps.

     ![Artefact](./media/contoso-migration-refactor-web-app-sql/pipeline6.png)

7. Ils cliquent sur **Mises en production** > **+Nouveau pipeline**.

    ![Nouveau pipeline](./media/contoso-migration-refactor-web-app-sql/pipeline7.png)

8. Ils sélectionnent le modèle de déploiement Azure App Service.

    ![Modèle Azure App Service](./media/contoso-migration-refactor-web-app-sql/pipeline8.png)

9. Ils nomment le pipeline de mise en production **ContosoSmartHotel360Refactor** et spécifient le nom de l’application web WCF (SHWCF-EUS2) pour le nom de la **phase**.

    ![Environnement](./media/contoso-migration-refactor-web-app-sql/pipeline9.png)

10. Sous les phases, ils cliquent sur **1 travail, 1 tâche** pour configurer le déploiement du service WCF.

    ![Déployer WCF](./media/contoso-migration-refactor-web-app-sql/pipeline10.png)

11. Ils vérifient que l’abonnement est sélectionné et autorisé, puis sélectionnent le **Nom de l’App Service**.

     ![Sélectionner un App Service](./media/contoso-migration-refactor-web-app-sql/pipeline11.png)

12. Dans le pipeline > **Artefacts**, ils sélectionnent **+Ajouter un artefact**, puis ils choisissent de générer avec le pipeline **ContosoSmarthotel360Refactor**.

     ![Créer](./media/contoso-migration-refactor-web-app-sql/pipeline12.png)

15. Ils cliquent sur l’éclair qui se trouve sur l’artefact coché pour activer le déclencheur de déploiement continu.

     ![Éclair](./media/contoso-migration-refactor-web-app-sql/pipeline13.png)

16. Le déclencheur de déploiement continu doit être défini sur **Activé**.

    ![Déploiement continu activé](./media/contoso-migration-refactor-web-app-sql/pipeline14.png) 

17. À présent, ils repassent à la phase «1 travail, 1 tâche », puis cliquent sur **Déployer Azure App Service**.

    ![Déployer App Service](./media/contoso-migration-refactor-web-app-sql/pipeline15.png)

18. Sous **Sélectionner un fichier ou dossier**, ils recherchent le fichier **SmartHotel.Registration.Wcf.zip** qui a été créé lors de la génération, puis ils cliquent sur **Enregistrer**.

    ![Enregistrer WCF](./media/contoso-migration-refactor-web-app-sql/pipeline16.png)

19. Ils cliquent sur **Pipeline** > **Phases** **+Ajouter** afin d’ajouter un environnement pour **SHWEB-EUS2**. Ils sélectionnent un autre déploiement Azure App Service.

    ![Ajouter un environnement](./media/contoso-migration-refactor-web-app-sql/pipeline17.png)

20. Ils répètent ensuite le processus pour publier l’application web (**SmartHotel.Registration.Web.zip**) sur l’application web correcte.

    ![Publier l’application web](./media/contoso-migration-refactor-web-app-sql/pipeline18.png)

21. Une fois qu’il est enregistré, le pipeline de mise en production apparaît comme suit.

     ![Résumé du pipeline de mise en production](./media/contoso-migration-refactor-web-app-sql/pipeline19.png)

22. Ils reviennent à **Build**, puis cliquent sur **Déclencheurs** > **Activer l’intégration continue**. Ceci active le pipeline pour que, quand des modifications sont validées dans le code, une build complète et une mise en production soient effectuées.

    ![Activer l’intégration continue](./media/contoso-migration-refactor-web-app-sql/pipeline20.png)

23. Ils cliquent sur **Enregistrer et mettre en file d’attente** pour exécuter le pipeline complet. Une nouvelle build est déclenchée, qui à son tour crée la première mise en production de l’application sur Azure App Service.

    ![Enregistrer le pipeline](./media/contoso-migration-refactor-web-app-sql/pipeline21.png)

24. Les administrateurs Contoso peuvent suivre le processus du pipeline de build et de mise en production depuis Azure DevOps. Une fois la build terminée, la mise en production commence.

    ![Générer et mettre en production l’application](./media/contoso-migration-refactor-web-app-sql/pipeline22.png)

25. Une fois que le pipeline a terminé, les deux sites ont été déployés et l’application est opérationnelle en ligne.

    ![Terminer la mise en production](./media/contoso-migration-refactor-web-app-sql/pipeline23.png)

À ce stade, la migration de l’application vers Azure a réussi.



## <a name="clean-up-after-migration"></a>Nettoyer après la migration

Après la migration, Contoso doit effectuer les étapes de nettoyage suivantes :  

- Supprimer les machines virtuelles locales de l’inventaire vCenter.
- Supprimer les machines virtuelles des travaux de sauvegarde locale.
- Mettre à jour la documentation interne afin qu’elle indique les nouveaux emplacements pour l’application SmartHotel360. Afficher la base de données en cours d’exécution dans la base de données Azure SQL et le front-end en cours d’exécution dans les deux applications web.
- Passer en revue toutes les ressources qui interagissent avec les machines virtuelles désactivées, et mettre à jour les paramètres ou la documentation appropriés afin de refléter la nouvelle configuration.


## <a name="review-the-deployment"></a>Examiner le déploiement

Avec les ressources migrées dans Azure, Contoso doit rendre sa nouvelle infrastructure entièrement opérationnelle et la sécuriser.

### <a name="security"></a>Sécurité

- Contoso doit vérifier que sa nouvelle base de données **SmartHotel-Registration** est sécurisée. [Plus d’informations](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview)
- En particulier, Contoso doit mettre à jour les applications web pour qu’elles utilisent SSL avec des certificats.

### <a name="backups"></a>Sauvegardes

- Contoso doit examiner les besoins de sauvegarde de la base de données Azure SQL. [Plus d’informations](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups)
- Contoso doit aussi découvrir comment gérer les sauvegardes et les restaurations de SQL Database. [Découvrez plus d’informations](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups) sur les sauvegardes automatiques.
- Contoso doit envisager d’implémenter des groupes de basculement afin de fournir un basculement régional pour la base de données. [Plus d’informations](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview)
- Pour la résilience de l’application web, Contoso doit envisager son déploiement dans les régions principales Est des États-Unis 2 et Centre des États-Unis. Contoso peut configurer Traffic Manager pour garantir le basculement en cas de pannes régionales.

### <a name="licensing-and-cost-optimization"></a>Gestion des licences et optimisation des coûts

- Une fois toutes les ressources déployées, Contoso doit affecter des balises Azure en fonction de la [planification de son infrastructure](contoso-migration-infrastructure.md#set-up-tagging).
- Toutes les licences sont intégrées dans le coût des services PaaS que Contoso consomme. Cela sera déduit de l’Accord Entreprise.
- Contoso va activer Azure Cost Management sous licence de Cloudyn, une filiale de Microsoft. Il s’agit d’une solution de gestion des coûts multicloud qui aide à utiliser et à gérer Azure ainsi que d’autres ressources cloud.  [En savoir plus](https://docs.microsoft.com/azure/cost-management/overview) sur Azure Cost Management.

## <a name="conclusion"></a>Conclusion

Cet article décrit comment Contoso a refactorisé l’application SmartHotel360 dans Azure en migrant la machine virtuelle du frontend de l’application vers deux applications Azure Web Apps. La base de données de l’application a été migrée vers une base de données Azure SQL.






