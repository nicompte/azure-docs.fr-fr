---
title: Notes de publication - Services Speech
titlesuffix: Azure Cognitive Services
description: Consultez un journal constamment mis à jour des futures versions, des améliorations, des correctifs de bogues et des problèmes connus pour les services Azure Speech.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: wolfma
ms.custom: seodec18
ms.openlocfilehash: f22b0fcac6099482addfcf56a20e0e828866326e
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65606356"
---
# <a name="release-notes"></a>Notes de publication

## <a name="speech-sdk-151"></a>Speech SDK 1.5.1

Il s’agit d’une version de correctif de bogue et affecte uniquement le Kit de développement natif/managé. Il n’affecte pas la version JavaScript du SDK.

**Résolution des bogues**

* Corriger FromSubscription lorsqu’il est utilisé avec la Transcription de Conversation.
* Corriger le bogue dans le mot clé tirant pour assistant virtuel vocal en premier.


## <a name="speech-sdk-150-2019-may-release"></a>Speech SDK 1.5.0 : Mise en production mai 2019

**Nouvelles fonctionnalités**

* Fonctionnalités de word (mot clé spotting/KWS) de mise en éveil est désormais disponible pour Windows et Linux. Les fonctionnalités KWS peuvent fonctionnent avec n’importe quel type de microphone, officielle KWS prennent en charge, toutefois, est limité aux tableaux microphone figurant actuellement dans le matériel Azure Kinect DK ou les appareils de Speech SDK.
* Fonctionnalités d’indicateur de phrase sont disponible via le Kit de développement. Vous pourrez trouver plus d’informations [ici](how-to-phrase-lists.md).
* Fonctionnalités de transcription de conversation sont disponible via le Kit de développement. Voir [ici](conversation-transcription-service.md).
* Ajouter la prise en charge pour les assistants virtuels de voix en premier en utilisant le canal Direct vocale de ligne.

**Exemples**

* Ajout d’exemples pour les nouvelles fonctionnalités ou de nouveaux services pris en charge par le Kit de développement.

**Améliorations/Modifications**

* Ajouté différentes propriétés de module de reconnaissance pour ajuster le comportement de service ou le résultat de service (par exemple, le masquage des grossièretés et autres).
* Vous pouvez maintenant configurer le module de reconnaissance via les propriétés de configuration standard, même si vous avez créé le module de reconnaissance `FromEndpoint`.
* Objective-c : `OutputFormat` propriété a été ajoutée à SPXSpeechConfiguration.
* Le Kit de développement logiciel prend désormais en charge Debian 9 comme une distribution Linux.

**Résolution des bogues**

* Correction d’un problème où la ressource de l’orateur a été détruite trop tôt dans la synthèse vocale.
## <a name="speech-sdk-142"></a>Speech SDK 1.4.2

Il s’agit d’une version de correctif de bogue et affecte uniquement le Kit de développement natif/managé. Il n’affecte pas la version JavaScript du SDK.

## <a name="speech-sdk-141"></a>Speech SDK 1.4.1

Il s’agit d’une version JavaScript uniquement. Aucune fonctionnalité n’a été ajoutée. Les correctifs suivants ont été appliqués :

* Pack de web empêcher le chargement de l’agent proxy https.

## <a name="speech-sdk-140-2019-april-release"></a>Speech SDK 1.4.0 : Version d’avril 2019

**Nouvelles fonctionnalités** 

* Le Kit de développement logiciel prend désormais en charge le service de synthèse vocale comme une version bêta. Il est pris en charge sur Windows et Linux Desktop à partir de C++ et C#. Pour plus d’informations, consultez le [vue d’ensemble de synthèse vocale](text-to-speech.md#get-started-with-text-to-speech).
* Le Kit de développement logiciel prend désormais en charge les fichiers audio MP3 et Opus/OGG en tant que fichiers d’entrée de flux de données. Cette fonctionnalité est uniquement disponible sur Linux à partir de C++ et C# et est actuellement en version bêta (plus de détails [ici](how-to-use-codec-compressed-audio-input-streams.md)).
* Le Speech SDK pour Java, .NET core, C++ et Objective-C ont acquis une prise en charge de macOS. La prise en charge de Objective-C pour macOS est actuellement en version bêta.
* iOS : Le Speech SDK pour iOS (Objective-C) est désormais également publié avec un CocoaPod.
* JavaScript : Prise en charge de microphone non définis par défaut comme périphérique d’entrée.
* JavaScript : Prise en charge de proxy pour Node.js.

**Exemples**

* Exemples d’utilisation du SDK de reconnaissance vocale avec C++ et Objective-C sur macOS ont été ajoutées.
* Exemples qui illustrent l’utilisation du service de synthèse vocale ont été ajoutées.

**Améliorations/Modifications**

* Python : Les propriétés supplémentaires des résultats de reconnaissance sont désormais exposées la `properties` propriété.
* Pour la prise en charge du débogage et de développement supplémentaires, vous pouvez rediriger les informations de journalisation et de diagnostics du kit SDK dans un fichier journal (plus de détails [ici](how-to-use-logging.md)).
* JavaScript : Améliorer les performances de traitement audio.

**Résolution des bogues**

* Mac/iOS: Correction d’un bogue qui a conduit à une attente longue lors d’une connexion au Service de reconnaissance vocale n’a pas pu être établie.
* Python : améliorer la gestion des erreurs pour les arguments dans les rappels de Python.
* JavaScript : Un état incorrect fixe reporting pour la reconnaissance vocale a pris fin RequestSession.

## <a name="speech-sdk-131-2019-february-refresh"></a>Speech SDK 1.3.1 : Réactualisation de février de 2019

Il s’agit d’une version de correctif de bogue et affecte uniquement le Kit de développement natif/managé. Il n’affecte pas la version JavaScript du SDK.

**Correctif de bogue**

* Correction d’une fuite de mémoire lors de l’utilisation de saisie par microphone. Stream en fonction ou un fichier d’entrée n’est pas affectée.

## <a name="speech-sdk-130-2019-february-release"></a>Kit de développement logiciel (SDK) de reconnaissance vocale 1.3.0 : Version de février 2019

**Nouvelles fonctionnalités**

* Le SDK Speech prend en charge la sélection du microphone d’entrée via la classe AudioConfig. Cela vous permet de transmettre les données audio aux Services de reconnaissance vocale à partir d’un microphone par défaut. Pour plus d’informations, consultez la documentation décrivant [sélection du périphérique d’entrée audio](how-to-select-audio-input-devices.md). Cette fonctionnalité n’est pas encore disponible à partir de JavaScript.
* Le SDK Speech prend désormais en charge Unity dans une version bêta. Fournir des commentaires via la section de problème dans le [référentiel d’exemples GitHub](https://aka.ms/csspeech/samples). Cette version prend en charge Unity sur Windows x86 et x64 (applications de bureau autonome ou plateforme Windows universelle) et Android (ARM32/64, x86). Des informations supplémentaires sont disponibles dans notre [Démarrage rapide Unity](quickstart-csharp-unity.md).
* Le fichier `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` (fourni dans les versions précédentes) n’est plus nécessaire. La fonctionnalité est maintenant intégrée dans le SDK core.


**Exemples**

Le nouveau contenu suivant est disponible dans notre [dépôt d’exemples](https://aka.ms/csspeech/samples) :

* Exemples supplémentaires pour AudioConfig.FromMicrophoneInput.
* Exemples Python supplémentaires pour la reconnaissance de l’intention et la traduction.
* Exemples Python supplémentaires pour la reconnaissance de l’intention et la traduction.
* Exemples Java supplémentaires pour la traduction avec une sortie audio.
* Nouvel exemple pour l’utilisation de l’[API REST de transcription Batch](batch-transcription.md).

**Améliorations/Modifications**

* Python
  * Vérification de paramètres améliorée et messages d’erreur dans SpeechConfig.
  * Ajouter la prise en charge de l’objet Connect.
  * Prise en charge de Python 32 bits (x86) sur Windows.
  * Le SDK Speech pour Python n’est plus en version bêta.
* iOS
  * Le SDK est désormais basé sur le SDK iOS version 12.1.
  * Le SDK prend désormais en charge iOS 9.2 et versions ultérieures.
  * Améliorer la documentation de référence et corriger plusieurs noms de propriété.
* JavaScript
  * Ajouter la prise en charge de l’objet Connect.
  * Ajouter des fichiers de définition de type pour JavaScript en offre groupée
  * Prise en charge initiale et implémentation des conseils.
  * Retourne la collection de propriétés avec le service JSON dans le cadre de la reconnaissance
* Les DLL Windows contiennent à présent une vraie ressource de version.
* Si vous créez un module de reconnaissance `FromEndpoint` vous pouvez ajouter des paramètres directement à l’URL de point de terminaison. À l’aide de `FromEndpoint` vous ne pouvez pas configurer le module de reconnaissance via les propriétés de configuration standard.

**Résolution des bogues**

* Le nom d’utilisateur et le mot de passe du proxy vides n’étaient pas été traités correctement. Avec cette version, si vous définissez un nom d’utilisateur et un mot de passe proxy en tant que chaîne vide, ils ne seront pas soumis lors de la connexion au proxy.
* Les SessionId créés par le SDK n’étaient pas toujours vraiment aléatoires pour certains langages&nbsp;/ environnements. Ajouté à l’initialisation du Générateur d’aléatoires pour résoudre ce problème.
* Amélioration de la gestion du jeton d’autorisation. Si vous souhaitez utiliser un jeton d’autorisation, spécifiez-le dans le SpeechConfig et ne renseignez pas la clé d’abonnement. Créez ensuite le module de reconnaissance comme d’habitude.
* Dans certains cas, la connexion de l’objet n’a pas été libéré correctement. Ce problème est à présent résolu.
* L’exemple JavaScript a été corrigé de façon à prendre en charge la sortie audio pour la synthèse de traduction également dans Safari.

## <a name="speech-sdk-121"></a>Kit de développement logiciel (SDK) Speech 1.2.1

Il s’agit d’une version JavaScript uniquement. Aucune fonctionnalité n’a été ajoutée. Les correctifs suivants ont été appliqués :

* Déclenchement du fin de flux au niveau de turn.end, et non pas de speech.end.
* Correction d’un bogue dans la pompe audio qui ne planifiait pas le prochain envoi si l’envoi en cours échouait.
* Correction de la reconnaissance continue avec le jeton d’authentification.
* Correction de bogue pour un module de reconnaissance / des points de terminaison différents.
* Améliorations de la documentation.

## <a name="speech-sdk-120-2018-december-release"></a>SDK Speech 1.2.0 : Version de décembre 2018

**Nouvelles fonctionnalités**

* Python
  * La version bêta de la prise en charge de Python (3.5 et au-delà) est disponible avec cette version. Pour plus d’informations, consultez here](quickstart-python.md).
* JavaScript
  * Le SDK Speech pour JavaScript est open source. Le code source est disponible sur [GitHub](https://github.com/Microsoft/cognitive-services-speech-sdk-js).
  * Nous prenons désormais en charge Node.js. Pour plus d’informations, [consultez cette page](quickstart-js-node.md).
  * La restriction sur la longueur des sessions audio a été supprimée. La reconnexion se produit automatiquement à l’arrière-plan.
* Objet Connection
  * Du module de reconnaissance, vous pouvez accéder à un objet de connexion. Cet objet vous permet de lancer explicitement la connexion au service et de vous abonner à des événements de connexion et de déconnexion.
    (Cette fonctionnalité n’est pas encore disponible à partir de JavaScript et Python.)
* Prise en charge d’Ubuntu 18.04.
* Android
  * Prise en charge de ProGuard activée durant la génération d’APK.

**Améliorations**

* Améliorations apportées à l’utilisation des threads internes afin de réduire le nombre de threads, de verrous et de mutex.
* Amélioration des rapports d’erreurs et des informations sur les erreurs. Dans plusieurs cas, les messages d’erreur n'ont pas été propagées avant toute chose.
* Mise à jour des dépendances de développement dans JavaScript pour utiliser des modules à jour.

**Résolution des bogues**

* Résolution des fuites de mémoire causés par une incompatibilité de type dans RecognizeAsync.
* Dans certains cas, des exceptions étaient divulguées.
* Résolution des fuites de mémoire dans les arguments d’événement de traduction.
* Résolution d’un problème de verrouillage à la suite d’une reconnexion dans les sessions de longue durée.
* Correction d’un problème qui pourrait entraîner manquant du résultat final des traductions ayant échouées.
* C# : Si une opération asynchrone n’était pas attendue dans le thread principal, le module de reconnaissance pouvait être supprimé avant la fin de la tâche asynchrone.
* Java : Résolution d’un problème entraînant un blocage de la machine virtuelle Java.
* Objective-C : Résolution d’un mappage d’enum ; RecognizedIntent était retourné à la place de RecognizingIntent.
* JavaScript : Définition du format de sortie par défaut « simple » dans SpeechConfig.
* JavaScript : Suppression d’une incohérence au niveau des propriétés sur l’objet config entre JavaScript et d’autres langages.

**Exemples**

* Mise à jour et correction de plusieurs exemples (par exemple sortie voix pour la traduction, etc.).
* Ajout d’exemples Node.js dans le [dépôt d’exemples](https://aka.ms/csspeech/samples).

## <a name="speech-sdk-110"></a>SDK Speech 1.1.0

**Nouvelles fonctionnalités**

* Prise en charge d'Android x86/x64.
* Prise en charge de proxy : Dans l’objet SpeechConfig, vous pouvez maintenant appeler une fonction pour définir les informations de proxy (nom d’hôte, port, nom d’utilisateur et mot de passe). Cette fonctionnalité n'est pas encore disponible sous iOS.
* Amélioration du code d’erreur et des messages. Si une reconnaissance a renvoyé une erreur, celle-ci a déjà défini `Reason` (dans l’événement annulé) ou `CancellationDetails` (dans le résultat de la reconnaissance) sur `Error`. L’événement annulé contient maintenant deux membres supplémentaires, `ErrorCode` et `ErrorDetails`. Si le serveur a renvoyé des informations d’erreur supplémentaires avec l’erreur signalée, elles sont désormais disponibles dans les nouveaux membres.

**Améliorations**

* Ajout d'une vérification supplémentaire dans la configuration du module de reconnaissance, et ajout d'un message d’erreur supplémentaire.
* Amélioration de la gestion des longs silences au milieu d’un fichier audio.
* Package NuGet : pour les projets .NET Framework, il empêche toute génération avec une configuration AnyCPU.

**Résolution des bogues**

* Correction de plusieurs exceptions détectées dans les modules de reconnaissance. En outre, les exceptions sont interceptées et converties en événement annulé.
* Correction d'une fuite de mémoire dans la gestion des propriétés.
* Correction d’un bogue dans lequel un fichier d’entrée audio pouvait bloquer le module de reconnaissance.
* Correction d’un bogue dans lequel des événements pouvaient être reçus après un événement d’arrêt de session.
* Correction de certaines conditions de concurrence dans le thread.
* Correction d’un problème de compatibilité avec iOS qui pouvait entraîner un blocage.
* Améliorations de la stabilité dans la prise en charge du microphone Android.
* Correction d’un bogue dans lequel un module de reconnaissance de JavaScript ignorait la langue de reconnaissance.
* Correction d’un bogue qui empêchait de définir EndpointId (dans certains cas) dans JavaScript.
* Modification de l'ordre des paramètres dans AddIntent dans JavaScript, et ajout de la signature AddIntent JavaScript manquante.

**Exemples**

* Ajout d’un exemple C++ et C# pour la diffusion en continu dans l’[exemple de référentiel](https://aka.ms/csspeech/samples).

## <a name="speech-sdk-101"></a>SDK Speech 1.0.1

Améliorations de la fiabilité et résolution des bogues :

* Correction d’une erreur irrécupérable potentielle due à une condition de concurrence lors de la suppression du module de reconnaissance
* Correction d’une erreur irrécupérable potentielle en cas de propriétés non définies.
* Vérification supplémentaire des erreurs et des paramètres.
* Objective-C : correction d’une erreur irrécupérable possible provoquée par le remplacement d’un nom dans une chaîne NSString.
* Objective-C : réglage de la visibilité de l’API.
* JavaScript : correction des événements et de leurs charges utiles.
* Améliorations de la documentation.

Dans notre [exemple de référentiel](https://aka.ms/csspeech/samples), un nouvel échantillon pour JavaScript a été ajouté.

## <a name="cognitive-services-speech-sdk-100-2018-september-release"></a>SDK Cognitive Services Speech 1.0.0 : version de septembre 2018

**Nouvelles fonctionnalités**

* Prise en charge d’Objective-C sur iOS. Découvrez le [guide de démarrage rapide sur Objective-C pour iOS](quickstart-objectivec-ios.md).
* Prise en charge de JavaScript dans le navigateur. Découvrez le [guide de démarrage rapide JavaScript](quickstart-js-browser.md).

**Dernières modifications**

* Avec cette version, un nombre de modifications avec rupture est introduit.
  Vérifiez [cette page](https://aka.ms/csspeech/breakingchanges_1_0_0) pour plus d’informations.

## <a name="cognitive-services-speech-sdk-060-2018-august-release"></a>Cognitive Services Speech SDK 0.6.0 : version d’août 2018

**Nouvelles fonctionnalités**

* Les applications UWP créées à partir du SDK Speech peuvent désormais passer le Kit de certification des applications Windows (WACK).
  Consultez le [guide de démarrage rapide UWP](quickstart-csharp-uwp.md).
* Prise en charge de .NET Standard 2.0 sur Linux (Ubuntu 16.04 x64).
* Expérimental : prise en charge de Java 8 sur Windows (64 bits) et Linux (Ubuntu 16.04 x64).
  Consultez le [guide de démarrage rapide Java Runtime Environment](quickstart-java-jre.md).

**Changement fonctionnel**

* Exposition d’informations supplémentaires sur les erreurs de connexion

**Dernières modifications**

* Sur Java (Android), la fonction `SpeechFactory.configureNativePlatformBindingWithDefaultCertificate` ne requiert plus aucun paramètre de chemin d’accès. Le chemin est désormais automatiquement détecté sur toutes les plateformes prises en charge.
* L’élément get-accessor de la propriété `EndpointUrl` dans Java et C# a été supprimé.

**Résolution des bogues**

* Dans Java, le résultat de la synthèse audio sur le module de reconnaissance de traduction est maintenant implémenté.
* Correction d’un bogue qui pouvait provoquer l’inactivité des threads et un plus grand nombre de sockets ouverts et inutilisés
* Correction d’un problème qui provoquait l’arrêt d’une reconnaissance de longue durée au milieu d’une transmission
* Correction d’une condition de concurrence lors de l’arrêt du module de reconnaissance.

## <a name="cognitive-services-speech-sdk-050-2018-july-release"></a>SDK Cognitive Services Speech 0.5.0 : version de juillet 2018

**Nouvelles fonctionnalités**

* Prise en charge de la plateforme Android (API 23 : Android 6.0 Marshmallow ou supérieur). Consultez le [Démarrage rapide Android](quickstart-java-android.md).
* Prise en charge de .NET Standard 2.0 sous Windows. Consultez le [Démarrage rapide .NET Core](quickstart-csharp-dotnetcore-windows.md).
* Expérimental : Prise en charge d’UWP sur Windows (version 1709 ou ultérieure).
  * Consultez le [guide de démarrage rapide UWP](quickstart-csharp-uwp.md).
  * Remarque : les applications UWP générées avec le kit SDK Speech ne réussissent pas encore le test du Kit de certification des applications Windows (WACK).
* Prise en charge des reconnaissances de longue durée avec la reconnexion automatique

**Modifications fonctionnelles**

* `StartContinuousRecognitionAsync()` prend en charge les reconnaissances de longue durée.
* Le résultat des reconnaissances contient davantage de champs. Ils sont décalés par rapport au début de l’audio et de la durée (tous les deux en cycles) du texte reconnu et des valeurs supplémentaires représentant l’état de la reconnaissance, par exemple, `InitialSilenceTimeout` et `InitialBabbleTimeout`.
* Prise en charge d’AuthorizationToken pour la création d’instances Data Factory.

**Dernières modifications**

* Événements de reconnaissance : le type d’événement NoMatch a été fusionné avec l’événement Error.
* Le SpeechOutputFormat du langage C# a été renommé OutputFormat pour s’aligner sur le C++.
* Le type de retour de certaines méthodes de l’interface `AudioInputStream` a été légèrement modifié :
   * Dans Java, la méthode `read` retourne désormais `long` au lieu de `int`.
   * Dans C#, la méthode `Read` retourne désormais `uint` au lieu de `int`.
   * Dans C++, les méthodes `Read` et `GetFormat` retournent désormais `size_t` au lieu de `int`.
* C++ : les instances de flux d’entrée audio peuvent maintenant être passées comme `shared_ptr`.

**Résolution des bogues**

* Correction des valeurs de retour incorrectes dans les résultats lorsque `RecognizeAsync()` expire.
* La dépendance aux bibliothèques Media Foundation Windows a été supprimée. Le SDK utilise désormais les API Core Audio.
* Correction de la documentation : ajout de la page [régions](regions.md) pour répertorier les régions prises en charge.

**Problème connu**

* Le SDK Speech pour Android ne signale pas les résultats de la synthèse vocale pour la traduction. Ce problème sera corrigé dans la prochaine version.

## <a name="cognitive-services-speech-sdk-040-2018-june-release"></a>SDK Cognitive Services Speech 0.4.0 : version de juin 2018

**Modifications fonctionnelles**

- AudioInputStream

  Un module de reconnaissance peut désormais consommer un flux en tant que source audio. Pour plus d’informations, consultez ce [guide pratique](how-to-use-audio-input-streams.md).

- Format de sortie détaillé

  Lorsque vous créez un `SpeechRecognizer`, vous pouvez demander le format de sortie `Detailed` ou `Simple`. Le `DetailedSpeechRecognitionResult` contient un score de confiance, le texte reconnu, la forme lexicale brute, la forme normalisée et la forme normalisée avec les blasphèmes masqués.

**Modification critique**

- `SpeechRecognitionResult.Text` a été remplacé par `SpeechRecognitionResult.RecognizedText` pour le langage C#.

**Résolution des bogues**

- Correction d’un problème de rappel possible dans la couche USP qui se produisait lors de l’arrêt.

- Si un module de reconnaissance a utilisé un fichier d’entrée audio, il a été placé sur le descripteur de fichier plus longtemps que nécessaire.

- Suppression de plusieurs blocages entre la pompe de messages et le module de reconnaissance.

- Expiration du délai de déclenchement d’un résultat `NoMatch` lors de la réponse du service.

- Les bibliothèques Media Foundation Windows sont chargées en différé. Cette bibliothèque est nécessaire uniquement pour l’entrée du microphone.

- La vitesse de chargement de données audio est limitée à environ deux fois la vitesse audio d’origine.

- Désormais, les noms d’assemblys C# .NET dans Windows sont forts.

- Correction de la documentation : `Region` est obligatoire pour créer un module de reconnaissance.

D’autres exemples ont été ajoutés et sont constamment mis à jour. Pour obtenir la dernière série d’exemples, accédez au [dépôt GitHub d’exemples pour le SDK Speech](https://aka.ms/csspeech/samples).

## <a name="cognitive-services-speech-sdk-0212733-2018-may-release"></a>SDK Cognitive Services Speech 0.2.12733 : version de mai 2018

Cette version est la première préversion publique du SDK Speech de Cognitive Services.
