---
title: Détection d’objets - Vision par ordinateur
titleSuffix: Azure Cognitive Services
description: Découvrez les concepts liés à la fonctionnalité de détection d’objet de l’API vision par ordinateur - utilisation et les limites.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 012ab849c926de332da55361c79c76c5a1311169
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60368037"
---
# <a name="detect-common-objects-in-images"></a>Détecter des objets dans des images

La détection d’objets est similaire au [balisage](concept-tagging-images.md), mais l’API retourne les coordonnées de cadre englobant (en pixels) pour chaque objet trouvé. Par exemple, si une image contient un chien, un chat et une personne, l’opération de détection liste ces objets ainsi que leurs coordonnées dans l’image. Vous pouvez utiliser cette fonctionnalité pour traiter les relations entre les objets dans une image. Il vous permet également de déterminer s’il existe plusieurs instances de la même balise dans une image.

L’API Détection applique des balises en fonction des objets ou éléments vivants identifiés dans l’image. Il n’existe actuellement aucune relation formelle entre la taxonomie de balises et la taxonomie de détection d’objet. À un niveau conceptuel, l’API détecter la recherche uniquement les objets et les choses de la vie, tandis que l’API de la balise peut également inclure des termes contextuelles telles que « intérieur », qui ne peut pas être localisés avec zones englobantes.

## <a name="object-detection-example"></a>Exemple de détection d’objet

La réponse JSON suivante illustre ce que retourne Vision par ordinateur lors de la détection d’objets dans l’exemple d’image.

![Une femme utilisant un appareil Microsoft Surface dans une cuisine](./Images/windows-kitchen.jpg)

```json
{
   "objects":[
      {
         "rectangle":{
            "x":730,
            "y":66,
            "w":135,
            "h":85
         },
         "object":"kitchen appliance",
         "confidence":0.501
      },
      {
         "rectangle":{
            "x":523,
            "y":377,
            "w":185,
            "h":46
         },
         "object":"computer keyboard",
         "confidence":0.51
      },
      {
         "rectangle":{
            "x":471,
            "y":218,
            "w":289,
            "h":226
         },
         "object":"Laptop",
         "confidence":0.85,
         "parent":{
            "object":"computer",
            "confidence":0.851
         }
      },
      {
         "rectangle":{
            "x":654,
            "y":0,
            "w":584,
            "h":473
         },
         "object":"person",
         "confidence":0.855
      }
   ],
   "requestId":"a7fde8fd-cc18-4f5f-99d3-897dcd07b308",
   "metadata":{
      "width":1260,
      "height":473,
      "format":"Jpeg"
   }
}
```

## <a name="limitations"></a>Limites

Il est important de noter les limitations de la détection d’objets afin d’éviter ou atténuer les effets de faux négatifs (objets manquées) et les détails limités.

* Les objets ne sont généralement pas détectés s’ils sont petits (moins de 5 % de l’image).
* Objets ne sont généralement pas détectées si leur disposition étroitement ensemble (une pile d’assiettes, par exemple).
* Les objets ne sont pas différenciés par marque ou nom de produit (différents types de sodas sur une étagère de magasin, par exemple). Toutefois, vous pouvez obtenir des informations sur les marques figurant sur une image à l'aide de la fonctionnalité [Détection de marque](concept-brand-detection.md).

## <a name="use-the-api"></a>Utilisation de l’API

La fonctionnalité de détection d'objet fait partie de l'API [Analyser l'image](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa). Vous pouvez appeler cette API via un SDK natif ou via des appels REST. Inclure `Objects` dans le **visualFeatures** paramètre de requête. Ensuite, lorsque vous obtenez la réponse JSON complète, simplement analyser la chaîne pour le contenu de la `"objects"` section.

* [Démarrage rapide : Analyser une image (SDK .NET)](./quickstarts-sdk/csharp-analyze-sdk.md)
* [Démarrage rapide : Analyser une image (API REST)](./quickstarts/csharp-analyze.md)