---
title: Compétence Reconnaissance d’entités de la recherche cognitive - Recherche Azure
description: Extrayez les différents types d’entités du texte dans un pipeline de recherche cognitive dans Recherche Azure.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: f05161dbbfd9293cd7b1cbf447bb7ca1c313250c
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65023441"
---
#    <a name="entity-recognition-cognitive-skill"></a>Compétence cognitive Reconnaissance d’entités

La compétence **Reconnaissance d’entités** extrait les entités de différents types du texte. Cette compétence utilise les modèles Machine Learning fournis par [Analyse de texte](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview) dans Cognitive Services.

> [!NOTE]
> Comme vous développez étendue en augmentant la fréquence de traitement, l’ajout de plusieurs documents, ou ajoutez plusieurs algorithmes d’intelligence artificielle, vous devrez [attacher une ressource Cognitive Services facturable](cognitive-search-attach-cognitive-services.md). Des frais sont applicables durant l’appel des API dans Cognitive Services ainsi que pour l’extraction d’images durant la phase d’extraction du contenu des documents du service Recherche Azure. L’extraction de texte à partir des documents est gratuite.
>
> L’exécution de compétences intégrées est facturée existant [Cognitive Services paie-sous-vous accédez prix](https://azure.microsoft.com/pricing/details/cognitive-services/). Image de tarification d’extraction est décrit sur le [page de tarification de Azure Search](https://go.microsoft.com/fwlink/?linkid=2042400).


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.EntityRecognitionSkill

## <a name="data-limits"></a>Limites de données
La taille maximale d’un enregistrement est de 50 000 caractères selon `String.Length`. Si vous devez subdiviser vos données avant de les envoyer à l’extracteur de phrases clés, envisagez d’utiliser la [compétence Fractionnement de texte](cognitive-search-skill-textsplit.md).

## <a name="skill-parameters"></a>Paramètres de la compétence

Les paramètres respectent la casse et sont tous facultatifs.

| Nom du paramètre     | Description |
|--------------------|-------------|
| Catégories    | Tableau des catégories à extraire.  Types de catégorie possibles : `"Person"`, `"Location"`, `"Organization"`, `"Quantity"`, `"Datetime"`, `"URL"`, `"Email"`. Si aucune catégorie n’est précisée, tous les types sont retournés.|
|defaultLanguageCode |  Code de langue du texte d’entrée. Langues prises en charge : `de, en, es, fr, it`.|
|minimumPrecision | Non utilisé. Réservé pour un usage futur. |
|includeTypelessEntities | Lorsque la valeur est true si le texte contient une entité connue, mais ne peut pas être classé dans les catégories prises en charge, il est retourné dans le cadre du champ de sortie complexe `"entities"`. 
Il s’agit d’entités qui sont bien connues, mais non classées dans le cadre des prise en charge actuelles « catégories ». Par exemple « Windows 10 » est une entité connue (produit), mais « Produits » ne sont pas dans les catégories actuellement prises en charge. La valeur par défaut est `false`. |


## <a name="skill-inputs"></a>Entrées de la compétence

| Nom d’entrée      | Description                   |
|---------------|-------------------------------|
| languageCode  | facultatif. La valeur par défaut est `"en"`.  |
| text          | Texte à analyser.          |

## <a name="skill-outputs"></a>Sorties de la compétence

> [!NOTE]
> toutes les catégories d’entités ne sont pas prises en charge pour toutes les langues. Uniquement _en_, _es_ prennent en charge l’extraction des types `"Quantity"`, `"Datetime"`, `"URL"`, `"Email"`.

| Nom de sortie     | Description                   |
|---------------|-------------------------------|
| persons      | Tableau de chaînes représentant chacune le nom d’une personne. |
| Emplacements  | Tableau de chaînes représentant chacune un lieu. |
| organizations  | Tableau de chaînes représentant chacune une organisation. |
| quantities  | Tableau de chaînes représentant chacune une quantité. |
| dateTimes  | Tableau de chaînes représentant chacune une valeur DateTime (telle qu’elle apparaît dans le texte). |
| urls | Tableau de chaînes représentant chacune une URL. |
| emails | Tableau de chaînes représentant chacune un e-mail. |
| namedEntities | Tableau de types complexes contenant les champs suivants : <ul><li>category</li> <li>valeur (le nom de l’entité réelle)</li><li>le décalage (l’emplacement où elle a été trouvée dans le texte) ;</li><li>confidence (non utilisé pour l’instant ; sera défini sur la valeur -1)</li></ul> |
| entities | Tableau de types complexes contenant des informations détaillées sur les entités extraites du texte, avec les champs suivants <ul><li> name (nom réel de l’entité ; il représente une forme « normalisée »)</li><li> wikipediaId</li><li>wikipediaLanguage</li><li>wikipediaUrl (lien vers la page Wikipedia relative à l’entité)</li><li>bingId</li><li>type (catégorie de l’entité reconnue)</li><li>subType (disponible uniquement pour certaines catégories ; il offre une vue plus précise du type d’entité)</li><li> matches (collection complexe contenant)<ul><li>text (texte brut pour l’entité)</li><li>offset (emplacement où cela a été trouvé)</li><li>length (longueur du texte brut d’entité)</li></ul></li></ul> |

##  <a name="sample-definition"></a>Exemple de définition

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
    "categories": [ "Person", "Email"],
    "defaultLanguageCode": "en",
    "includeTypelessEntities": true,
    "inputs": [
      {
        "name": "text",
        "source": "/document/content"
      }
    ],
    "outputs": [
      {
        "name": "persons",
        "targetName": "people"
      },
      {
        "name": "emails",
        "targetName": "contact"
      },
      {
        "name": "entities"
      }
    ]
  }
```
##  <a name="sample-input"></a>Exemple d’entrée

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "text": "Contoso corporation was founded by John Smith. They can be reached at contact@contoso.com",
             "languageCode": "en"
           }
      }
    ]
}
```

##  <a name="sample-output"></a>Exemple de sortie

```json
{
  "values": [
    {
      "recordId": "1",
      "data" : 
      {
        "persons": [ "John Smith"],
        "emails":["contact@contoso.com"],
        "namedEntities": 
        [
          {
            "category":"Person",
            "value": "John Smith",
            "offset": 35,
            "confidence": -1
          }
        ],
        "entities":  
        [
          {
            "name":"John Smith",
            "wikipediaId": null,
            "wikipediaLanguage": null,
            "wikipediaUrl": null,
            "bingId": null,
            "type": "Person",
            "subType": null,
            "matches": [{
                "text": "John Smith",
                "offset": 35,
                "length": 10
            }]
          },
          {
            "name": "contact@contoso.com",
            "wikipediaId": null,
            "wikipediaLanguage": null,
            "wikipediaUrl": null,
            "bingId": null,
            "type": "Email",
            "subType": null,
            "matches": [
            {
                "text": "contact@contoso.com",
                "offset": 70,
                "length": 19
            }]
          },
          {
            "name": "Contoso",
            "wikipediaId": "Contoso",
            "wikipediaLanguage": "en",
            "wikipediaUrl": "https://en.wikipedia.org/wiki/Contoso",
            "bingId": "349f014e-7a37-e619-0374-787ebb288113",
            "type": null,
            "subType": null,
            "matches": [
            {
                "text": "Contoso",
                "offset": 0,
                "length": 7
            }]
          }
        ]
      }
    }
  ]
}
```


## <a name="error-cases"></a>Cas d’erreur
Si le code de langue du document n’est pas pris en charge, une erreur est retournée et aucune entité n’est extraite.

## <a name="see-also"></a>Voir aussi

+ [Compétences prédéfinies](cognitive-search-predefined-skills.md)
+ [Guide pratique pour définir un ensemble de compétences](cognitive-search-defining-skillset.md)
