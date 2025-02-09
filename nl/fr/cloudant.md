---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-10"

keywords: nodejs storage, nodejs cloudant, nodejs iam, initialize sdk nodejs, test nodejs app, dbaas nodejs, nodejs-cloudant, store documents nodejs

subcollection: nodejs

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Stockage de documents dans {{site.data.keyword.cloud_notm}}
{: #cloudant}

{{site.data.keyword.cloudantfull}} est une base de données sous forme de service (Database as a Service, DBaaS) orientée document, qui stocke les données en tant que documents JSON. {{site.data.keyword.cloudant_short_notm}} a été conçu en gardant à l'esprit l'évolutivité, la haute disponibilité et la durabilité, et elle est facile à configurer pour l'utilisation d'applications Node.js. Elle est fournie avec une large gamme d'options d'indexation incluant notamment MapReduce, {{site.data.keyword.cloudant_short_notm}} Query, l'indexation en texte intégral et l'indexation géospatiale. Ses fonctions de réplication facilitent la synchronisation des données entre les clusters de base de données, les ordinateurs de bureau et les périphériques mobiles.
{:shortdesc}

Pour plus d'informations, voir [{{site.data.keyword.cloudant_short_notm}} Basics](/docs/services/Cloudant/basics?topic=cloudant-ibm-cloudant-basics).

## Avant de commencer
{: #prereqs-cloudant}

Assurez-vous que les prérequis suivants sont satisfaits :
 * [Nodejs-cloudant ](https://github.com/cloudant/nodejs-cloudant){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe") 2.3.0+, bibliothèque client.
 * Vous devez disposer d'un [compte {{site.data.keyword.cloud}}](https://cloud.ibm.com/registration){: new_window}![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe").
 * Pour accéder à {{site.data.keyword.cloudant_short_notm}}, vos devez créer un service dans le [tableau de bord {{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/resources){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe"), puis démarrer le tableau de bord {{site.data.keyword.cloudant_short_notm}} depuis cette instance de service. 
 * Les fragments de code dans ces instructions utilisent l'authentification IAM.
 
### Activation d'IAM avec {{site.data.keyword.cloudant_short_notm}}
{: #enable_IAM-cloudant}

Seules les nouvelles instances de service {{site.data.keyword.cloudant_short_notm}} peuvent être utilisées avec {{site.data.keyword.cloud_notm}} IAM.

Toutes les instances de service {{site.data.keyword.cloudant_short_notm}} nouvelles sont activées pour utiliser {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) lors de la mise à disposition. Lorsque vous mettez une nouvelle instance à disposition à partir du catalogue {{site.data.keyword.cloud_notm}}, choisissez la méthode d'authentification **Use only IAM**. Ce mode signifie que seules les données d'identification IAM sont fournies par la liaison de service et la génération de données d'identification. Pour plus d'informations, visitez [{{site.data.keyword.cloud_notm}} Identity and Access Management (IAM)](/docs/services/Cloudant/guides?topic=cloudant-ibm-cloud-identity-and-access-management-iam-#ibm-cloud-identity-and-access-management-iam-).

## Etape 1. Création d'une instance de {{site.data.keyword.cloudant_short_notm}}
{: #create-instance-cloudant}

Lorsque vous créez une instance de {{site.data.keyword.cloudant_short_notm}}, vous créez également la base de données.

1. Connectez-vous à votre compte {{site.data.keyword.cloud_notm}}.
2. Depuis le [tableau de bord {{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/resources){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe"), cliquez sur **Créer une ressource**. Le catalogue {{site.data.keyword.cloud_notm}} s'ouvre.
3. Dans le [catalogue {{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/catalog){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe"), sélectionnez la catégorie **Bases de données** puis cliquez sur {{site.data.keyword.cloudant_short_notm}}. La page de configuration de service s'ouvre.
4. Renseignez les zones suivantes :
  * **Nom du service** - Entrez un nom pour votre instance de service ou utilisez le nom prédéfini.
  * **Sélectionnez une région/un emplacement où effectuer le déploiement** - Sélectionnez une région dans laquelle déployer votre service.
  * **Sélectionnez un groupe de ressources** - Sélectionnez un groupe de ressources ou acceptez le groupe par défaut.
  * **Méthodes d'authentification disponibles** - Sélectionnez **Utiliser uniquement IAM** comme méthode d'authentification.
5. Sélectionnez votre plan de tarification, puis cliquez sur **Créer**. La page de votre instance de service s'affiche.
6. Pour créer des données d'identification du service, procédez comme suit :
  1. Depuis le menu de navigation, sélectionnez **Données d'identification du service**.
  2. Cliquez sur **Nouvelles données d'identification**. La page Ajouter de nouvelles données d'identification s'affiche.
  3. Dans la page Ajouter de nouvelles données d'identification, renseignez les zones, puis cliquez sur **Ajouter**. Les nouvelles données d'identification du service sont ajoutées à l'instance de service.
  4. Si vous souhaitez afficher les détails des données d'identification du service, cliquez sur **Afficher les données d'identification** dans la colonne **Actions** des nouvelles données d'identification.
7. Depuis le menu de navigation, sélectionnez **Gérer**, puis cliquez sur **Lancer le tableau de bord Cloudant**.
8. Depuis le menu de navigation, cliquez sur l'icône **Bases de données**.
9. Cliquez sur **Créer une base de données**, indiquez un nom de base de données, puis cliquez sur **Créer.** La page de votre base de données s'affiche.

Si vous souhaitez voir les informations connexes concernant la mise à disposition d'une instance du service {{site.data.keyword.cloud_notm}}, voir [Creating an IBM Cloudant instance on IBM Cloud tutorial](/docs/services/Cloudant/tutorials?topic=cloudant-creating-an-ibm-cloudant-instance-on-ibm-cloud#creating-a-cloudant-nosql-db-instance-on-ibm-cloud).

## Etape 2. Installation du logiciel SDK
{: #install-cloudant}

<!--From github.com/cloudant/nodejs-cloudant#installation-and-usage-->

Commencez votre propre projet Node.js et définissez ce travail comme votre dépendance. En d'autres termes, placez {{site.data.keyword.cloudant_short_notm}} dans vos dépendances package.json. Utilisez le gestionnaire de package [npm](https://nodejs.org/){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe") depuis la ligne de commande pour installer le logiciel SDK :
```
npm install --save @cloudant/cloudant
```
{: codeblock}

Notez que votre fichier `package.json` comporte à présent le package Cloudant.

## Etape 3. Initialisation du logiciel SDK
{: #initialize-cloudant}

Une fois que vous avez initialisé le logiciel SDK dans votre application, vous pouvez utiliser {{site.data.keyword.cloudant_short_notm}} pour stocker les données. Pour initialiser votre connexion, entrez vos données d'identification et fournissez une fonction appelée à exécuter quand tout est prêt.

1. Chargez la bibliothèque client en ajoutant la définition `require` suivante à votre fichier `server.js`.
  ```js
  var Cloudant = require('@cloudant/cloudant');
  ```
  {: codeblock}

2. Initialisez la bibliothèque client en fournissant les données d'identification. Utilisez le plug-in `iamauth` pour créer un client de base de données avec une clé d'API IAM. 
  ```js
  var cloudant = new Cloudant({ url: 'https://examples.cloudant.com', plugins: { iamauth: { iamApiKey: 'xxxxxxxxxx' } } });
  ```
  {: codeblock}

3. Répertoriez les bases de données en ajoutant le code suivant à votre fichier `server.js`.
  ```javascript
  cloudant.db.list(function(err, body) {
  body.forEach(function(db) {
    console.log(db);
    });
  });
  ```
  {: codeblock}

`Cloudant` en majuscules correspond au package que vous chargez via `require()`. `cloudant` en minuscules est la connexion authentifiée à votre service {{site.data.keyword.cloudant_short_notm}}.
{: tip}

### Gestion des données à l'aide d'opérations de base
{: #basic_operations-cloudant}
<!--Borrowed from https://github.com/cloudant/nodejs-cloudant/blob/master/example/crud.js-->

Ces opérations de base illustrent les actions consistant à créer, lire, mettre à jour et supprimer vos documents à l'aide du client initialisé.

#### Création d'un document
```js
var createDocument = function(callback) {
  console.log("Creating document 'mydoc'");
  // specify the id of the document so you can update and delete it later
  db.insert({ _id: 'mydoc', a: 1, b: 'two' }, function(err, data) {
    console.log('Error:', err);
    console.log('Data:', data);
    callback(err, data);
  });
};
```
{: codeblock}

#### Lecture d'un document
```js
var readDocument = function(callback) {
  console.log("Reading document 'mydoc'");
  db.get('mydoc', function(err, data) {
    console.log('Error:', err);
    console.log('Data:', data);
    // keep a copy of the doc so you know its revision token
    doc = data;
    callback(err, data);
  });
};
```
{: codeblock}

#### Mise à jour d'un document
```js
var updateDocument = function(callback) {
  console.log("Updating document 'mydoc'");
  // make a change to the document, using the copy we kept from reading it back
  doc.c = true;
  db.insert(doc, function(err, data) {
    console.log('Error:', err);
    console.log('Data:', data);
    // keep the revision of the update so we can delete it
    doc._rev = data.rev;
    callback(err, data);
  });
};
```
{: codeblock}

#### Suppression d'un document
```js
var deleteDocument = function(callback) {
  console.log("Deleting document 'mydoc'");
  // supply the id and revision to be deleted
  db.destroy(doc._id, doc._rev, function(err, data) {
    console.log('Error:', err);
    console.log('Data:', data);
    callback(err, data);
  });
};
```
{: codeblock}

## Etape 4. Test de votre application
{: #test-cloudant}

Tout est correctement configuré ? Il est temps de tester !

1. Exécutez votre application, en veillant à démarrer l'initialisation et les opérations respectives, comme la création d'un document.
2. Dans le [tableau de bord {{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/resources){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe"), cliquez sur l'instance de service {{site.data.keyword.cloudant_short_notm}} précédemment créée. Lorsque l'instance de service s'affiche, cliquez sur **Lancer le tableau de bord Cloudant**.
3. Dans le tableau de bord {{site.data.keyword.cloudant_short_notm}}, sélectionnez la base de données dans laquelle vous avez créé les nouveaux documents.

Vous rencontrez des problèmes ? Consultez la [Référence d'API {{site.data.keyword.cloudant_short_notm}}](/docs/services/Cloudant?topic=cloudant-api-reference-overview).

## Etapes suivantes
{: #next-cloudant notoc}

Félicitations ! Vous avez ajouté un niveau de persistance sécurisé à votre application. Poursuivez sur votre lancée en essayant l'une des options suivantes :

* Affichez le code source pour [{{site.data.keyword.cloudant_short_notm}} SDK for Node.js ](https://github.com/cloudant/nodejs-cloudant){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe").
* Consultez l'[exemple de code pour les opérations de base de données et de document ](https://github.com/cloudant/nodejs-cloudant/tree/master/example){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe").
* Les kits de démarrage (Starter Kits) constituent l'une des façons les plus rapides d'utiliser les fonctions d'{{site.data.keyword.cloud}}. Affichez les kits de démarrage disponibles dans le tableau de bord [Mobile developer dashboard ](https://cloud.ibm.com/developer/mobile/dashboard){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe"). Téléchargez le code. Exécutez l'application !
* Pour en savoir plus et tirer pleinement parti des fonctions offertes par {{site.data.keyword.cloudant_short_notm}}, [consultez la documentation](/docs/services/Cloudant?topic=cloudant-getting-started).
