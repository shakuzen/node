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

# Dokumente in {{site.data.keyword.cloud_notm}} speichern
{: #cloudant}

{{site.data.keyword.cloudantfull}} ist ein dokumentorientiertes DBaaS-Angebot (DBaaS, Database as a Service). Es speichert Daten als Dokumente im JSON-Format. {{site.data.keyword.cloudant_short_notm}} ist für Skalierbarkeit, Hochverfügbarkeit und Dauerhaftigkeit ausgelegt und kann leicht für die Verwendung in Node.js-Anwendungen konfiguriert werden. Es enthält eine Vielzahl von Indexierungsoptionen, die MapReduce, {{site.data.keyword.cloudant_short_notm}} Query, Volltextindexierung und geografisch-räumlich Indexierung umfassen. Mit den Replikationsfunktionen ist es einfach, Daten zwischen Datenbankclustern, Desktop-PCs und mobilen Geräten synchron zu halten.
{:shortdesc}

Weitere Informationen finden Sie in [{{site.data.keyword.cloudant_short_notm}}-Grundlagen](/docs/services/Cloudant/basics?topic=cloudant-ibm-cloudant-basics). 

## Vorbereitende Schritte
{: #prereqs-cloudant}

Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:
 * [Nodejs-cloudant ](https://github.com/cloudant/nodejs-cloudant){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") 2.3.0+-Clientbibliothek.
 * Sie müssen über ein [{{site.data.keyword.cloud}}-Konto ](https://cloud.ibm.com/registration){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") verfügen.
 * Für den Zugriff auf {{site.data.keyword.cloudant_short_notm}} müssen Sie einen Service im [{{site.data.keyword.cloud_notm}}-Dashboard](https://cloud.ibm.com/resources){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") erstellen und anschließend das {{site.data.keyword.cloudant_short_notm}}-Dashboard in dieser Serviceinstanz starten. 
 * Die Code-Snippets in diesen Anweisungen verwenden die IAM-Authentifizierung.
 
### IAM mit {{site.data.keyword.cloudant_short_notm}} aktivieren
{: #enable_IAM-cloudant}

Nur neue {{site.data.keyword.cloudant_short_notm}}-Serviceinstanzen können mit {{site.data.keyword.cloud_notm}} IAM verwendet werden.

Alle neuen {{site.data.keyword.cloudant_short_notm}}-Serviceinstanzen sind bei der Bereitstellung für die Verwendung von {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) aktiviert. Wenn Sie eine neue Instanz aus dem {{site.data.keyword.cloud_notm}}-Katalog bereitstellen, wählen Sie die Authentifizierungsmethode **Nur IAM verwenden** aus. Dieser Modus bedeutet, dass nur IAM-Berechtigungsnachweise durch die Servicebindung und die Generierung von Berechtigungsnachweisen bereitgestellt werden. Weitere Informationen finden Sie in [{{site.data.keyword.cloud_notm}} Identity and Access Management (IAM)](/docs/services/Cloudant/guides?topic=cloudant-ibm-cloud-identity-and-access-management-iam-#ibm-cloud-identity-and-access-management-iam-).

## Schritt 1. Instanz von {{site.data.keyword.cloudant_short_notm}} erstellen
{: #create-instance-cloudant}

Wenn Sie eine Instanz von {{site.data.keyword.cloudant_short_notm}} erstellen, erstellen Sie auch die Datenbank.

1. Melden Sie sich bei Ihrem {{site.data.keyword.cloud_notm}}-Konto an.
2. Klicken Sie im [{{site.data.keyword.cloud_notm}}-Dashboard ](https://cloud.ibm.com/resources){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") auf **Ressource erstellen**. Der {{site.data.keyword.cloud_notm}}-Katalog wird geöffnet.
3. Wählen Sie im [{{site.data.keyword.cloud_notm}}-Katalog](https://cloud.ibm.com/catalog){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") die Kategorie **Datenbanken** aus und klicken Sie dann auf {{site.data.keyword.cloudant_short_notm}}. Die Seite mit der Servicekonfiguration wird geöffnet.
4. Vervollständigen Sie die Informationen in den folgenden Feldern:
  * **Servicename**: Geben Sie entweder einen Namen für Ihre Serviceinstanz ein oder verwenden Sie den voreingestellten Namen.
  * **Region/Standort für die Bereitstellung auswählen**: Wählen Sie eine Region für die Bereitstellung des Service aus.
  * **Ressourcengruppe auswählen**: Wählen Sie eine Ressourcengruppe aus oder übernehmen Sie die Standardeinstellung.
  * **Verfügbare Authentifizierungsmethoden**: Wählen Sie **Nur IAM verwenden** für die Authentifizierungsmethode aus.
5. Wählen Sie Ihren Preistarif aus und klicken Sie dann auf **Erstellen**. Die Seite für Ihre Serviceinstanz wird geöffnet.
6. Führen Sie die folgenden Schritte aus, um einen Serviceberechtigungsnachweis zu erstellen:
  1. Wählen Sie im Navigationsmenü die Option **Serviceberechtigungsnachweise** aus.
  2. Klicken Sie auf **Neuer Berechtigungsnachweis**. Daraufhin wird die Seite 'Neuen Berechtigungsnachweis hinzufügen' geöffnet.
  3. Vervollständigen Sie auf der Seite 'Neuen Berechtigungsnachweis hinzufügen' die Felder und klicken Sie dann auf **Hinzufügen**. Der neue Serviceberechtigungsnachweis wird zur Serviceinstanz hinzugefügt.
  4. Wenn Sie die Details des Serviceberechtigungsnachweises anzeigen möchten, klicken Sie in der Spalte **Aktionen** des neuen Berechtigungsnachweises auf **Berechtigungsnachweise anzeigen**.
7. Wählen Sie im Navigationsmenü die Option **Verwalten** aus und klicken Sie anschließend auf **Cloudant-Dashboard starten**.
8. Klicken Sie im Navigationsmenü auf das Symbol für **Datenbanken**.
9. Klicken Sie auf **Datenbank erstellen**, geben Sie einen Datenbanknamen an und klicken Sie anschließend auf **Erstellen**. Die Datenbankseite wird geöffnet.

Wenn Sie zugehörige Informationen zur Bereitstellung einer Instanz des {{site.data.keyword.cloud_notm}}-Service anzeigen möchten, finden Sie weitere Informationen im  [Lernprogramm zum Erstellen einer IBM Cloudant-Instanz in IBM Cloud](/docs/services/Cloudant/tutorials?topic=cloudant-creating-an-ibm-cloudant-instance-on-ibm-cloud#creating-a-cloudant-nosql-db-instance-on-ibm-cloud).

## Schritt 2. SDK installieren
{: #install-cloudant}

<!--From github.com/cloudant/nodejs-cloudant#installation-and-usage-->

Starten Sie mit Ihrem eigenen Node.js-Projekt und definieren Sie diese Arbeit als Ihre Abhängigkeit. Mit anderen Worten: Definieren Sie {{site.data.keyword.cloudant_short_notm}} als Abhängigkeit der Datei 'package.json'. Verwenden Sie den Paketmanager [npm](https://nodejs.org/){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") über die Befehlszeile, um das SDK zu installieren:
```
npm install --save @cloudant/cloudant
```
{: codeblock}

Beachten Sie, dass die Datei `package.json` nun das Cloudant-Paket enthält.

## Schritt 3. SDK initialisieren
{: #initialize-cloudant}

Nachdem Sie das SDK in Ihrer App initialisiert haben, können Sie {{site.data.keyword.cloudant_short_notm}} zum Speichern von Daten verwenden. Um Ihre Verbindung zu initialisieren, geben Sie Ihre Berechtigungsnachweise ein und geben Sie eine Callback-Funktion an, die ausgeführt werden soll, wenn alles bereit ist.

1. Laden Sie die Clientbibliothek, indem Sie die folgende `require`-Definition zur Datei `server.js` hinzufügen.
  ```js
  var Cloudant = require('@cloudant/cloudant');
  ```
  {: codeblock}

2. Initialisieren Sie die Clientbibliothek, indem Sie Ihre Berechtigungsnachweise bereitstellen. Verwenden Sie das `iamauth`-Plug-in, um einen Datenbankclient mit einem IAM-API-Schlüssel zu erstellen. 
  ```js
  var cloudant = new Cloudant({ url: 'https://examples.cloudant.com', plugins: { iamauth: { iamApiKey: 'xxxxxxxxxx' } } });
  ```
  {: codeblock}

3. Listen Sie die Datenbanken auf, indem Sie den folgenden Code zur Datei `server.js` hinzufügen.
  ```javascript
  cloudant.db.list(function(err, body) {
  body.forEach(function(db) {
    console.log(db);
    });
  });
  ```
  {: codeblock}

Das großgeschriebene `Cloudant` ist das Paket, das Sie mithilfe von `require()` laden. Das kleingeschriebene `cloudant` ist die authentifizierte Verbindung zu Ihrem {{site.data.keyword.cloudant_short_notm}}-Service.
{: tip}

### Daten mit Basisoperationen verwalten
{: #basic_operations-cloudant}
<!--Borrowed from https://github.com/cloudant/nodejs-cloudant/blob/master/example/crud.js-->

Diese Basisoperationen veranschaulichen die Aktionen zum Erstellen, Lesen, Aktualisieren und Löschen Ihrer Dokumente mithilfe des initialisierten Clients.

#### Ein Dokument erstellen
```js
var createDocument = function(callback) {
  console.log("Creating document 'mydoc'");
  // Geben Sie die ID des Dokuments an, damit Sie es später aktualisieren und löschen können
  db.insert({ _id: 'mydoc', a: 1, b: 'two' }, function(err, data) {
    console.log('Error:', err);
    console.log('Data:', data);
    callback(err, data);
  });
};
```
{: codeblock}

#### Ein Dokument lesen
```js
var readDocument = function(callback) {
  console.log("Reading document 'mydoc'");
  db.get('mydoc', function(err, data) {
    console.log('Error:', err);
    console.log('Data:', data);
    // Bewahren Sie eine Kopie des Dokuments auf, damit Sie dessen Überarbeitungstoken kennen
    doc = data;
    callback(err, data);
  });
};
```
{: codeblock}

#### Ein Dokument aktualisieren
```js
var updateDocument = function(callback) {
  console.log("Updating document 'mydoc'");
  // Ändern Sie das Dokument mithilfe der Kopie, die Sie beim Zurücklesen aufbewahrt hatten
  doc.c = true;
  db.insert(doc, function(err, data) {
    console.log('Error:', err);
    console.log('Data:', data);
    // Bewahren Sie die Überarbeitung der Aktualisierung auf, damit diese gelöscht werden kann
    doc._rev = data.rev;
    callback(err, data);
  });
};
```
{: codeblock}

#### Ein Dokument löschen
```js
var deleteDocument = function(callback) {
  console.log("Deleting document 'mydoc'");
  // Geben Sie die zu löschende ID und Überarbeitung an
  db.destroy(doc._id, doc._rev, function(err, data) {
    console.log('Error:', err);
    console.log('Data:', data);
    callback(err, data);
  });
};
```
{: codeblock}

## Schritt 4. App testen
{: #test-cloudant}

Ist alles richtig eingerichtet? Testen Sie es jetzt!

1. Führen Sie Ihre Anwendung aus und stellen Sie sicher, dass Sie die Initialisierung und die entsprechenden Operationen, wie das Erstellen eines Dokuments, starten.
2. Klicken Sie im [{{site.data.keyword.cloud_notm}}-Dashboard](https://cloud.ibm.com/resources){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") auf die {{site.data.keyword.cloudant_short_notm}}-Serviceinstanz, die Sie zuvor erstellt haben. Wenn die Serviceinstanz geöffnet wird, klicken Sie auf **Cloudant-Dashboard starten**.
3. Wählen Sie im {{site.data.keyword.cloudant_short_notm}}-Dashboard die Datenbank aus, in der Sie die neuen Dokumente erstellt haben.

Haben Sie Schwierigkeiten? Lesen Sie die [{{site.data.keyword.cloudant_short_notm}}-API-Referenz](/docs/services/Cloudant?topic=cloudant-api-reference-overview).

## Nächste Schritte
{: #next-cloudant notoc}

Gut gemacht! Sie haben Ihrer App ein Maß an sicherer Persistenz hinzugefügt. Machen Sie am besten gleich weiter, indem Sie eine der folgenden Optionen ausprobieren:

* Zeigen Sie die Quellcode für das [{{site.data.keyword.cloudant_short_notm}} SDK for Node.js ](https://github.com/cloudant/nodejs-cloudant){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") an.
* Sehen Sie sich den [Beispielcode für Datenbank- und Dokumentoperationen ](https://github.com/cloudant/nodejs-cloudant/tree/master/example){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") genauer an.
* Mit Starter-Kits können Sie die Funktionalität von {{site.data.keyword.cloud}} optimal und schnell nutzen. Zeigen Sie die verfügbaren Starter-Kits im [Mobile-Enwicklerdashboard ](https://cloud.ibm.com/developer/mobile/dashboard){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") an. Laden Sie den Code herunter. Führen Sie die App aus!
* Wenn Sie mehr zu den Funktionen von {{site.data.keyword.cloudant_short_notm}} und deren Verwendung erfahren möchten, [lesen Sie die Dokumentation](/docs/services/Cloudant?topic=cloudant-getting-started).
