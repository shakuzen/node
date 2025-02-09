---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-13"

keywords: deploy nodejs app, serverless nodejs, back-end nodejs, generated sdk nodejs, cloud foundry deploy nodejs, kubernetes deploy nodejs, virtual nodejs

subcollection: nodejs

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# Node.js-Apps bereitstellen und integrieren
{: #deploy_apps-nodejs}

Sie können Ihre Node.js-Apps mit einer Toolchain oder über die Befehlszeilenschnittstelle bereitstellen. Eine Toolchain ist eine Gruppe von Toolintegrationen. Die Befehlszeilenschnittstelle ist eine einfache Möglichkeit, Ihre Apps und Serviceinstanzen bereitzustellen. 

Weitere Informationen finden Sie im Abschnitt
[Apps bereitstellen](/docs/apps?topic=creating-apps-deploying-apps).

## Serverunabhängige Node.js-Apps bereitstellen
{: #serverless-nodejs}

Um die serverunabhängige Entwicklung zu vereinfachen, können Sie das IBM Functions as a Service-Angebot (FaaS) {{site.data.keyword.openwhisk}} verwenden. {{site.data.keyword.openwhisk_short}} ermöglicht die Ausführung von Anwendungslogik als Reaktion auf Ereignisse oder direkte Aufrufe von Web- oder mobilen Apps über HTTP ohne die Bereitstellung oder Verwaltung von Servern.

{{site.data.keyword.openwhisk_short}} führt Systemverwaltungstasks wie automatische Skalierung, Verfügbarkeitsmanagement und Wartung aus, wodurch sich Entwickler auf das Schreiben von Anwendungslogik konzentrieren können.

Weitere Informationen enthält der Abschnitt
[Serverunabhängige Apps
entwickeln](/docs/apps/deploying?topic=creating-apps-serverless).

## Back-End-Services mit einem generierten SDK integrieren
{: #backend_gensdk}

Mit dem {{site.data.keyword.IBM_notm}} SDK-Generator-Plug-in können Back-End-Services problemlos mit einem generierten SDK in Ihre App integriert werden. Wenn eine Änderung an einer REST-API vorgenommen wird, können Sie das SDK neu generieren und das alte SDK ersetzen, um ein nahtloses SDK-Upgrade durchführen zu können. Sie können die Befehlszeilenschnittstelle (CLI) auch in eine DevOps-Pipeline integrieren und sicherstellen, dass das SDK jedes Mal, wenn die App erstellt wird, mit der API-Spezifikation konsistent ist.

Weitere Informationen finden Sie unter
[Back-End-Services mit einem
generierten SDK integrieren](/docs/swift/backend?topic=swift-sdkgen-cli).

## In einem Kubernetes-Cluster bereitstellen
{: #deploy_kube}

Sie können erfahren, wie Sie den Kubernetes-Service von {{site.data.keyword.cloud_notm}} zur Bereitstellung einer containerisierten Node.js-App verwenden können, die Watson Tone Analyzer verwenden. Im vorliegenden Szenario verwendet eine fiktive PR-Firma den {{site.data.keyword.cloud_notm}}-Service, um ihre Pressemitteilungen zu analysieren und Feedback zum Ton ihrer Nachrichten zu erhalten. Weitere Informationen finden Sie im Lernprogramm [Apps in Kubernetes-Clustern bereitstellen](/docs/containers?topic=containers-cs_apps_tutorial). 

## In Cloud Foundry bereitstellen
{: #node-deploy-cf}

Mit dieser Option wird die cloudnative App bereitgestellt, ohne dass Sie die zugrunde liegende Infrastruktur verwalten müssen.

Wenn Sie die App in [{{site.data.keyword.cfee_full}}](/docs/cloud-foundry?topic=cloud-foundry-about) bereitstellen möchten, müssen Sie [Ihr {{site.data.keyword.cloud_notm}}-Konto vorbereiten](/docs/cloud-foundry?topic=cloud-foundry-prepare).

Wenn Ihr Konto über Zugriff auf {{site.data.keyword.cfee_full_notm}} verfügt, können Sie als Bereitstellertyp entweder **[Public Cloud](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf)** oder **[Enterprise Environment](/docs/cloud-foundry-public?topic=cloud-foundry-public-cfee)** auswählen, das zum Erstellen und Verwalten isolierter Umgebungen für das exklusive Hosting von Cloud Foundry-Anwendungen für Ihr Unternehmen verwendet werden kann.

## Auf einem virtuellen Server bereitstellen
{: #virtual_deploy}

Ein virtueller Service bietet einen höheren Grad an Transparenz, Vorhersagbarkeit und Automatisierung für alle Workloadtypen. Terraform wird verwendet, um Ihre Infrastruktur sicher und effizient zu erstellen, zu ändern und zu versionieren.

  VSI-Bereitstellung ist für einige Starter-Kits verfügbar. Wenn Sie diese Funktion verwenden möchten, rufen Sie das [{{site.data.keyword.cloud_notm}}-Dashboard](https://{DomainName}) auf und klicken Sie auf **App erstellen** in der Kachel **Apps**.
  {: note} 

Weitere Informationen finden Sie in [Auf einem virtuellen Server bereitstellen](/docs/vsi?topic=virtual-servers-deploying-to-a-virtual-server). 
