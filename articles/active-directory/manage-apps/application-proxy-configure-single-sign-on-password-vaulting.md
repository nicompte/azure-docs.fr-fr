---
title: Authentification unique auprès des applications avec le proxy d’application Azure AD | Microsoft Docs
description: Activez l’authentification unique pour vos applications locales publiées avec le proxy d’application Azure AD dans le portail Azure.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: celested
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2339ce23379ab51090d536ba071a42017f6941b5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60293074"
---
# <a name="password-vaulting-for-single-sign-on-with-application-proxy"></a>Authentification unique avec mise au coffre des mots de passe par le biais du proxy d’application

Le service Proxy d’application Azure Active Directory vous aide à améliorer la productivité en publiant des applications locales afin que les employés travaillant à distance puissent également y accéder. Dans le portail Azure, vous pouvez également configurer l’authentification unique auprès de ces applications. Vos utilisateurs n’ont qu’à s’authentifier auprès d’Azure AD pour accéder à votre application d’entreprise sans se reconnecter.

Le proxy d’application prend en charge plusieurs [modes d’authentification unique](what-is-single-sign-on.md#choosing-a-single-sign-on-method). L’authentification par mot de passe est destinée aux applications qui utilisent une combinaison nom d’utilisateur/mot de passe pour l’authentification. Lorsque vous configurez l’authentification par mot de passe pour votre application, vos utilisateurs ne doivent se connecter à l’application locale qu’une seule fois. Azure Active Directory stocke ensuite les informations de connexion et les fournit automatiquement à l’application lorsque vos utilisateurs y accèdent à distance. 

Vous devez avoir déjà publié et testé votre application avec le proxy d’application. Sinon, suivez les étapes de la rubrique [Publier des applications avec le Proxy d’application Azure AD](application-proxy-add-on-premises-application.md) avant de revenir ici. 

## <a name="set-up-password-vaulting-for-your-application"></a>Configurer le stockage de mots de passe pour votre application

1. Connectez-vous au [portail Azure](https://portal.azure.com) en tant qu’administrateur.
2. Sélectionnez **Azure Active Directory** > **Applications d’entreprise** > **Toutes les applications**.
3. Dans la liste, sélectionnez l’application pour laquelle vous voulez configurer l’authentification unique.  
4. Sélectionner **Authentification unique**.

   ![Sélectionner l’authentification unique](./media/application-proxy-configure-single-sign-on-password-vaulting/select-sso.png)

5. Pour le mode d’authentification unique, choisissez **Authentification par mot de passe**.
6. Pour l’URL de connexion, entrez l’URL de la page où les utilisateurs entrent leur nom d’utilisateur et leur mot de passe pour se connecter à votre application en dehors du réseau d’entreprise. Il peut s’agir de l’URL externe que vous avez créée quand vous avez publié l’application par le biais du proxy d’application. 

   ![Choisissez Authentification par mot de passe et entrez votre URL.](./media/application-proxy-configure-single-sign-on-password-vaulting/password-sso.png)

7. Sélectionnez **Enregistrer**.

<!-- Need to repro?
7. The page should tell you that a sign-in form was successfully detected at the provided URL. If it doesn't, select **Configure [your app name] Password Single Sign-on Settings** and choose **Manually detect sign-in fields**. Follow the instructions to point out where the sign-in credentials go. 
-->

## <a name="test-your-app"></a>Test de l'application

Accédez à l’URL externe que vous avez configurée pour l’accès à distance à votre application. Connectez-vous avec vos informations d’identification associées à cette application (ou celles d’un compte de test pour lequel vous avez configuré un accès). Une fois que vous êtes connecté, vous devez pouvoir quitter l’application et revenir sans entrer à nouveau vos informations d’identification. 

## <a name="next-steps"></a>Étapes suivantes

- En savoir plus sur les autres façons d’implémenter l’[authentification unique](what-is-single-sign-on.md)
- En savoir plus sur les [considérations de sécurité pour l’accès aux applications à distance avec le proxy d’application Azure AD](application-proxy-security.md)
