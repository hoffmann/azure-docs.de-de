---
title: Erste Schritte mit einem Beispiel
description: "Power BI Embedded, Verwenden von SDK zum Hinzufügen interaktiver Power BI-Berichte zu Ihrer Business Intelligence-Anwendung"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: d8a9ef78-ad4e-4bc7-9711-89172dc5c548
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 10/04/2016
ms.author: asaxton
translationtype: Human Translation
ms.sourcegitcommit: 830eb6627cae71f358b9790791b1d86f7c82c566
ms.openlocfilehash: 90207623273c3c8f959a07425e1bdf7324649067


---
# <a name="get-started-with-power-bi-embedded-sample"></a>Erste Schritte mit dem Beispiel zu Power BI Embedded
Mit **Microsoft Power BI Embedded**können Sie Power BI-Berichte direkt in webbasierte oder mobile Anwendungen integrieren. Dieser Artikel enthält eine Einführung in das Beispiel für die ersten Schritte mit **Power BI Embedded** .

Bevor wir fortfahren, sollten Sie die folgenden Ressourcen speichern. Diese Ressourcen sind hilfreich, wenn Sie Power BI-Berichte in die Beispiel-App (und auch in Ihre eigenen Apps) integrieren.

* [Beispiel für eine Dashboard-Web-App](http://go.microsoft.com/fwlink/?LinkId=761493)
* [Power BI Embedded-API-Referenz](https://msdn.microsoft.com/library/mt711493.aspx)
* [Power BI Embedded .NET SDK ](http://go.microsoft.com/fwlink/?LinkId=746472) (verfügbar über NuGet)

> [!NOTE]
> Bevor Sie das Beispiel zu den ersten Schritten mit Power BI Embedded konfigurieren und ausführen können, müssen Sie mindestens eine **Arbeitsbereichssammlung** in Ihrem Azure-Abonnement erstellen. Informationen zum Erstellen einer **Arbeitsbereichssammlung** im Azure-Portal finden Sie unter [Erste Schritte mit Power BI Embedded](power-bi-embedded-get-started.md).
>
>

## <a name="configure-the-sample-app"></a>Konfigurieren der Beispiel-App
Richten wir zunächst Schritt für Schritt die Visual Studio-Entwicklungsumgebung ein, um auf die Komponenten zuzugreifen, die zum Ausführen der Beispiel-App erforderlich sind.

1. Laden Sie das Beispiel [Power BI Embedded – Integrate a report into a web app](http://go.microsoft.com/fwlink/?LinkId=761493) (Integrieren eines Berichts in eine Web-App) von GitHub herunter, und extrahieren Sie die Datei.
2. Öffnen Sie **PowerBI embedded.sln** in Visual Studio. Möglicherweise müssen Sie den Befehl **Update-Package** in der NuGet-Paket-Manager-Konsole ausführen, um die Pakete zu aktualisieren, die in dieser Lösung verwendet werden.
3. Erstellen Sie die Projektmappe.
4. Führen Sie die Konsolen-App **ProvisionSample** aus. In der Beispiel-App-Konsole stellen Sie einen Arbeitsbereich bereit und importieren eine PBIX-Datei.
5. Wählen Sie zum Bereitstellen eines neuen **Arbeitsbereichs** Option 5 **Provision a new workspace in an existing workspace collection** aus.

    ![](media\\powerbi-embedded-get-started-sample\\console-option-5.png)
6. Geben Sie den Namen Ihrer **Arbeitsbereichssammlung** und den **Zugriffsschlüssel** ein. Sie finden diese Angaben im **Azure-Portal**. Weitere Informationen zum Abrufen Ihres **Zugriffsschlüssels**finden Sie unter [Anzeigen von Power BI-API-Zugriffsschlüsseln](power-bi-embedded-get-started.md#view-power-bi-api-access-keys) in „Erste Schritte mit Microsoft Power BI Embedded“.

    ![](media\\powerbi-embedded-get-started-sample\\azure-portal.png)
7. Kopieren und speichern Sie die neu erstellte **Arbeitsbereichs-ID** , um sie später in diesem Artikel zu verwenden. Nach der Erstellung ist die **Arbeitsbereichs-ID** im **Azure-Portal** zu finden.

    ![](media\\powerbi-embedded-get-started-sample\\workspace-id.png)
8. Zum Importieren einer PBIX-Datei in Ihren **Arbeitsbereich** wählen Sie Option **6. Importieren Sie die PBIX-Desktopdatei in einen vorhandenen Arbeitsbereich**. Falls Sie keine PBIX-Datei parat haben, können Sie das [PBIX-Beispiel „Retail Analysis“](http://go.microsoft.com/fwlink/?LinkID=780547) herunterladen.
9. Wenn Sie dazu aufgefordert werden, geben Sie einen Anzeigenamen für Ihr **Dataset**ein.

Sie sollten in etwa folgende Rückgabe erhalten:

````
Checking import state... Publishing
Checking import state... Succeeded
```

> [!NOTE]
> If your PBIX file contains any direct query connections, run option 7 to update the connection strings.
>
>

At this point, you have a Power BI PBIX report imported into your **Workspace**. Now, let's look at how to run the **Power BI Embedded** get started sample web app.

## Run the sample web app
The web app sample is a sample dashboard that renders reports imported into your **Workspace**. Here's how to configure the web app sample.

1. In the **PowerBI-embedded** Visual Studio solution, right click the **EmbedSample** web application, and choose **Set as StartUp project**.
2. In **web.config**, in the **EmbedSample** web application, edit the **appSettings**: **AccessKey**, **WorkspaceCollection** name, and **WorkspaceId**.

    ```
    <appSettings>
        <add key="powerbi:AccessKey" value="" />
        <add key="powerbi:ApiUrl" value="https://api.powerbi.com" />
        <add key="powerbi:WorkspaceCollection" value="" />
        <add key="powerbi:WorkspaceId" value="" />
    </appSettings>
    ```
3. Run the **EmbedSample** web application.

Once you run the **EmbedSample** web application, the left navigation panel should contain a **Reports** menu. To view the report you imported, expand **Reports**, and click a report. If you imported the [Retail Analysis Sample PBIX](http://go.microsoft.com/fwlink/?LinkID=780547), the sample web app would look like this:

![](media\\powerbi-embedded-get-started-sample\\power-bi-embedded-sample-left-nav.png)

After you click a report, the **EmbedSample** web application should look something this:

![](media\\powerbi-embedded-get-started-sample\\sample-web-app.png)

## Explore the sample code
The **Microsoft Power BI Embedded** sample is an example dashboard web app that shows you how to integrate **Power BI** reports into your app. It uses a Model-View-Controller (MVC) design pattern to demonstrate best practices. This section highlights parts of the sample code that you can explore within the **PowerBI-embedded** web app solution. The Model-View-Controller (MVC) pattern separates the modeling of the domain, the presentation, and the actions based on user input into three separate classes: Model, View, and Control. To learn more about MVC, see [Learn About ASP.NET](http://www.asp.net/mvc).

The **Microsoft Power BI Embedded** sample code is separated as follows. Each section includes the file name in the PowerBI-embedded.sln solution so that you can easily find the code in the sample.

> [!NOTE]
> This section is a summary of the sample code that shows how the code was written. To view the complete sample, please load the PowerBI-embedded.sln solution in Visual Studio.
>
>

### Model
The sample has a **ReportsViewModel** and **ReportViewModel**.

**ReportsViewModel.cs**: Represents Power BI Reports.

    public class ReportsViewModel
    {
        public List<Report> Reports { get; set; }
    }

**ReportViewModel.cs**: Represents a Power BI Report.

    public classReportViewModel
    {
        public IReport Report { get; set; }

        public string AccessToken { get; set; }
    }

### Connection string
The connection string must be in the following format:

```
Data Source=tcp:MyServer.database.windows.net,1433;Initial Catalog=MyDatabase
```

Using common server and database attributes will fail. For example: Server=tcp:MyServer.database.windows.net,1433;Database=MyDatabase,

### View
The **View** manages the display of Power BI **Reports** and a Power BI **Report**.

**Reports.cshtml**: Iterate over **Model.Reports** to create an **ActionLink**. The **ActionLink** is composed as follows:

| Part | Description |
| --- | --- |
| Title |Name of the Report. |
| QueryString |A link to the Report ID. |

    <div id="reports-nav" class="panel-collapse collapse">
        <div class="panel-body">
            <ul class="nav navbar-nav">
                @foreach (var report in Model.Reports)
                {
                    var reportClass = Request.QueryString["reportId"] == report.Id ? "active" : "";
                    <li class="@reportClass">
                        @Html.ActionLink(report.Name, "Report", new { reportId = report.Id })
                    </li>
                }
            </ul>
        </div>
    </div>

Report.cshtml: Set the **Model.AccessToken**, and the Lambda expression for **PowerBIReportFor**.

    @model ReportViewModel

    ...

    <div class="side-body padding-top">
        @Html.PowerBIAccessToken(Model.AccessToken)
        @Html.PowerBIReportFor(m => m.Report, new { style = "height:85vh" })
    </div>

### Controller
**DashboardController.cs**: Creates a PowerBIClient passing an **app token**. A JSON Web Token (JWT) is generated from the **Signing Key** to get the **Credentials**. The **Credentials** are used to create an instance of **PowerBIClient**. Once you have an instance of **PowerBIClient**, you can call GetReports() and GetReportsAsync().

CreatePowerBIClient()

    private IPowerBIClient CreatePowerBIClient()
    {
        var credentials = new TokenCredentials(accessKey, "AppKey");
        var client = new PowerBIClient(credentials)
        {
            BaseUri = new Uri(apiUrl)
        };

        return client;
    }

ActionResult Reports()

    public ActionResult Reports()
    {
        using (var client = this.CreatePowerBIClient())
        {
            var reportsResponse = client.Reports.GetReports(this.workspaceCollection, this.workspaceId);

            var viewModel = new ReportsViewModel
            {
                Reports = reportsResponse.Value.ToList()
            };

            return PartialView(viewModel);
        }
    }


Task<ActionResult> Report(string reportId)

    public async Task<ActionResult> Report(string reportId)
    {
        using (var client = this.CreatePowerBIClient())
        {
            var reportsResponse = await client.Reports.GetReportsAsync(this.workspaceCollection, this.workspaceId);
            var report = reportsResponse.Value.FirstOrDefault(r => r.Id == reportId);
            var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id);

            var viewModel = new ReportViewModel
            {
                Report = report,
                AccessToken = embedToken.Generate(this.accessKey)
            };

            return View(viewModel);
        }
    }

### Integrate a report into your app
Once you have a **Report**, you use an **IFrame** to embed the Power BI **Report**. Here is a code snippet from  powerbi.js in the **Microsoft Power BI Embedded** sample.

![](media\\powerbi-embedded-get-started-sample\\power-bi-embedded-iframe-code.png)

## Filter reports embedded in your application
You can filter an embedded report using a URL syntax. To do this, you add a **$filter** query string parameter with an **eq** operator to your iFrame src url with the filter specified. Here is the filter query syntax:

```
https://app.powerbi.com/reportEmbed
?reportId=d2a0ea38-...-9673-ee9655d54a4a&
$filter={tableName/fieldName}%20eq%20'{fieldValue}'
```

> [!NOTE]
> {tableName/fieldName} cannot include spaces or special characters. The {fieldValue} accepts a single categorical value.  
>
>

## See also
* [Common Microsoft Power BI Embedded scenarios](power-bi-embedded-scenarios.md)
* [Authenticating and authorizing in Power BI Embedded](power-bi-embedded-app-token-flow.md)



<!--HONumber=Nov16_HO3-->


