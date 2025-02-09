---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: cos nodejs, object storage nodejs, nodejs data, file storage nodejs, ibm-cos-sdk nodejs, creating object nodejs, downloading object nodejs, static nodejs

subcollection: nodejs

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Stockage d'un contenu statique dans Object Storage
{: #object-storage}

<!-- Sample Code for the SDK: https://github.com/ibm/ibm-cos-sdk-js#example-code -->

<!-- More sample code: https://cloud.ibm.com/docs/services/cloud-object-storage/libraries?topic=cloud-object-storage-node -->

<!-- Object storage tutorial under the Storing and sharing data topicgroup:
https://cloud.ibm.com/docs/services/cloud-object-storage?topic=cloud-object-storage-about -->

{{site.data.keyword.cos_full_notm}} est un composant fondamental du Cloud Computing et fournit de puissantes fonctionnalités aux développeurs Apple ainsi qu'à leurs applications. A la différence du stockage d'informations dans une hiérarchie de fichier (comme le stockage par blocs ou le stockage de fichiers), un conteneur d'objets se compose uniquement de fichiers et de leurs métadonnées. Ces fichiers sont stockés dans des collections appelées compartiments. Par définition, ces objets sont non modifiables, ce qui les rend parfaits pour les données comme les images, les vidéos et d'autres documents statiques. Pour que les données qui changent fréquemment ou sont des données relationnelles, vous pouvez utiliser le service de base de données [{{site.data.keyword.cloudant_short_notm}}](/docs/node?topic=nodejs-cloudant).

{{site.data.keyword.cos_short}} (COS) est système de stockage qui peut être utilisé pour le stockage de données non structurées à la fois flexibles, abordables et évolutives. Les données sont accessible via des logiciels SDK ou via l'interface utilisateur IBM. Vous pouvez utiliser {{site.data.keyword.cos_short}} pour accéder aux données non structurées via un portail en libre-service adossé à des API RESTful et des logiciels SDK.

## Avant de commencer
{: #prereqs-cos}

Assurez-vous que les prérequis suivants sont satisfaits :
1. Vous devez disposer d'un [compte {{site.data.keyword.cloud}}](https://cloud.ibm.com/registration){: new_window}![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe").
2. Vous devez disposer du logiciel [{{site.data.keyword.cos_short}} SDK for Node.js ](https://github.com/ibm/ibm-cos-sdk-js){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe").
3. Vous devez disposer de Node 4.x+.
4. Localisez les valeurs de clé de données d'identification à utiliser ultérieurement pour l'initialisation du kit SDK :

    * _**endpoint**_ - noeud final public de votre instance Cloud Object Storage. Ce noeud final est disponible dans le [tableau de bord {{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/resources){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe").
    * _**api-key**_ - Clé d'API qui est générée lors de la création des données d'identification du service. Un accès en écriture est nécessaire pour la création et la suppression d'exemples.
    * _**resource-instance-id**_ - ID ressource pour votre instance Cloud Object Storage. L'ID ressource est disponible via l'interface de ligne de commande [{{site.data.keyword.cloud_notm}}](/docs/cli?topic=cloud-cli-getting-started) ou le tableau de bord [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/resources){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe").

## Etape 1. Création d'une instance de {{site.data.keyword.cos_short}}
{: #create-instance-cos}

1. Dans le [catalogue {{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/catalog){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe"), sélectionnez la catégorie **Stockage** et cliquez sur {{site.data.keyword.cos_short}}. La page de configuration de service s'ouvre.
2. Donnez un nom à votre instance de service ou utilisez le nom prédéfini.
3. Sélectionnez votre plan de tarification, puis cliquez sur **Créer**. La page de votre instance Object Storage s'affiche.
4. Dans le menu de navigation, sélectionnez **Données d'identification du service**.
5. Dans la page Données d'identification du service, cliquez sur **Nouvelles données d'identification**.
6. Dans la page Ajouter de nouvelles données d'identification, vérifiez que le rôle est défini sur **Auteur,** puis cliquez sur **Ajouter.** Les nouvelles données d'identification sont créées et affichées dans la page Données d'identification du service.

## Etape 2. Installation du logiciel SDK
{: #install-cos}

Installez le logiciel {{site.data.keyword.cos_short}} SDK for Node.js en utilisant le gestionnaire de package [npm](https://nodejs.org/en/){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe") à partir de la ligne de commande :
```
npm install ibm-cos-sdk
```
{: pre}

## Etape 3. Initialisation du logiciel SDK
{: #initialize-cos}

Une fois que vous avez initialisé le logiciel SDK dans votre application, vous pouvez utiliser {{site.data.keyword.cos_short}} pour stocker les données. Initialisez votre connexion en indiquant vos données d'identification et en fournissant une fonction appelée à exécuter quand tout est prêt.

1. Chargez la bibliothèque client en ajoutant les définitions `require` suivantes à votre fichier `server.js`.
  ```js
  var objectStore = require('ibm-cos-sdk');
  ```
  {: codeblock}

2. Initialisez la bibliothèque client en fournissant les données d'identification.
  ```js
  var config = {
    endpoint: '<endpoint>',
    apiKeyId: '<api-key>',
    ibmAuthEndpoint: 'https://iam.ng.bluemix.net/oidc/token',
    serviceInstanceId: '<resource-instance-id>',
  };
  ```
  {: codeblock}

  Si vous avez besoin d'aide pour trouver les valeurs de clé de données d'identification pour votre application, consultez l'*étape 4* de la section [Avant de commencer](#prereqs-cos) pour plus de détails. Voir également [Données d'identification du service pour {{site.data.keyword.cos_short}}](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials).
  {: tip}

3. Ajoutez le code suivant à votre fichier `server.js`.
  ```js
  var cos = new objectStore(config);
  ```
  {: codeblock}

### Gestion des données avec des opérations de base
{: #basic_operations}
<!--Borrowed from https://github.com/ibm/ibm-cos-sdk-js#example-code-->

#### Création d'un compartiment
```js
function doCreateBucket() {
    console.log('Creating bucket');
    return cos.createBucket({
        Bucket: 'my-bucket',
        CreateBucketConfiguration: {
          LocationConstraint: 'us-standard'
        },
    }).promise();
}
```
{: codeblock}

#### Création, téléchargement et remplacement d'un objet
```js
function doCreateObject() {
    console.log('Creating object');
    return cos.putObject({
        Bucket: 'my-bucket',
        Key: 'foo',
        Body: 'bar'
    }).promise();
}
```
{: codeblock}

#### Téléchargement d'un objet
<!-- Verify this snippet with Nick when he returns from vacation -->
```js
function doGetObject() {
 console.log('Getting object');
 return cos.getObject({
   Bucket: 'my-bucket',
   Key: 'foo'
 }).createReadStream().pipe(fs.createWriteStream('./MyObject'));
}
```
{: codeblock}

#### Suppression d'un objet
```js
function doDeleteObject() {
    console.log('Deleting object');
    return cos.deleteObject({
        Bucket: 'my-bucket',
        Key: 'foo'
    }).promise();
}
```
{: codeblock}

Consultez la [documentation complète](/docs/services/cloud-object-storage?topic=cloud-object-storage-node) pour plus de détails sur les téléchargements à plusieurs parties, les fonctions de sécurité et d'autres opérations.

## Etape 4. Test de votre application
{: #test-cos}

Tout est correctement configuré ? Il est temps de tester !

1. Lancez cette application, en vous assurant de démarrer l'initialisation et les opérations respectives, comme la création d'un compartiment et l'ajout de données à ce dernier.
2. Retournez dans l'instance de service {{site.data.keyword.cos_short}} que vous avez préalablement créée dans votre navigateur Web, puis ouvrez le tableau de bord des services.
3. Sélectionnez le compartiment qui est utilisé afin de voir les objets nouvellement créés dans votre tableau de bord.

Vous rencontrez des problèmes ? Consultez les [informations de référence sur l'API {{site.data.keyword.cos_short}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api).

## Etapes suivantes
{: #next-cos notoc}

Félicitations ! Vous avez ajouté un niveau de persistance sécurisé à votre application. Poursuivez sur votre lancée en essayant l'une des options suivantes :

* Affichez le code source pour [{{site.data.keyword.cos_short}} SDK for Node.js ](https://github.com/ibm/ibm-cos-sdk-js){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe").
* Consultez l'[exemple de code pour les opérations de compartiment et d'objet ](https://github.com/ibm/ibm-cos-sdk-js#example-code){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe").
* Les kits de démarrage (Starter Kits) constituent l'une des façons les plus rapides d'utiliser les fonctions d'{{site.data.keyword.cloud_notm}}. Affichez les kits de démarrage disponibles dans le tableau de bord [Mobile developer dashboard ](https://cloud.ibm.com/developer/mobile/dashboard){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe"). Téléchargez le code. Exécutez l'application !
* Pour en savoir plus et tirer pleinement parti des fonctions offertes par {{site.data.keyword.cos_short}}, [consultez la documentation](/docs/services/cloud-object-storage?topic=cloud-object-storage-about).
