---
ms.openlocfilehash: 744df064e4fdc1bbf7821ae43a7b7878148902e9
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584616"
---
<!-- markdownlint-disable MD002 MD041 -->

Erstellen Sie zunächst eine Blazer-Webassembly-app.

1. Öffnen Sie die Befehlszeilenschnittstelle (CLI) in einem Verzeichnis, in dem Sie das Projekt erstellen möchten. Führen Sie den folgenden Befehl aus.

    ```Shell
    dotnet new blazorwasm --auth SingleOrg -o GraphTutorial
    ```

    Der `--auth SingleOrg` Parameter bewirkt, dass das generierte Projekt die Konfiguration für die Authentifizierung mit der Microsoft Identity-Plattform einschließt.

1. Nachdem das Projekt erstellt wurde, stellen Sie sicher, dass es funktioniert, indem Sie das aktuelle Verzeichnis in das **GraphTutorial** -Verzeichnis ändern und den folgenden Befehl in ihrer CLI ausführen.

    ```Shell
    dotnet watch run
    ```

1. Öffnen Sie den Browser, und navigieren Sie zu `https://localhost:5001` . Wenn alles funktioniert, sollte ein "Hello, World!" angezeigt werden. Nachricht.

> [!IMPORTANT]
> Wenn Sie eine Warnung erhalten, dass das Zertifikat für **localhost** nicht vertrauenswürdig ist, können Sie die .net-Kern-CLI verwenden, um das Entwicklungszertifikat zu installieren und ihm zu vertrauen. Anweisungen zu bestimmten Betriebssystemen finden Sie unter [Erzwingen von HTTPS in ASP.net Core](/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.1) .

## <a name="add-nuget-packages"></a>Hinzufügen von NuGet-Paketen

Bevor Sie fortfahren, installieren Sie einige zusätzliche NuGet-Pakete, die Sie später verwenden werden.

- [Microsoft.Graph](https://www.nuget.org/packages/Microsoft.Graph/) zum Aufrufen von Microsoft Graph
- [TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) für die Übersetzung von Windows-Zeitzonenbezeichnern in IANA-IDs.

1. Führen Sie die folgenden Befehle in der CLI aus, um die Abhängigkeiten zu installieren.

    ```Shell
    dotnet add package Microsoft.Graph --version 3.18.0
    dotnet add package TimeZoneConverter
    ```

## <a name="design-the-app"></a>Entwerfen der App

In diesem Abschnitt wird die grundlegende UI-Struktur der Anwendung erstellt.

1. Entfernen von Beispielseiten, die von der Vorlage generiert wurden. Löschen Sie die folgenden Dateien.

    - **./Pages/Counter.razor**
    - **./Pages/FetchData.razor**
    - **./Shared/SurveyPrompt.razor**
    - **./wwwroot/Sample-Data/weather.jsein**

1. Öffnen Sie **./wwwroot/index.html** , und fügen Sie den folgenden Code direkt **vor** dem Endtag hinzu `</body>` .

    :::code language="html" source="../demo/GraphTutorial/wwwroot/index.html" id="BootStrapJSSnippet":::

    Dadurch werden die [Bootstrap](https://getbootstrap.com/docs/4.5/getting-started/introduction/) -JavaScript-Dateien hinzugefügt.

1. Öffnen Sie **/wwwroot/CSS/app.CSS** , und fügen Sie den folgenden Code hinzu.

    :::code language="css" source="../demo/GraphTutorial/wwwroot/css/app.css" id="CssSnippet":::

1. Öffnen Sie **./Shared/NavMenu.Razor** , und ersetzen Sie den Inhalt durch Folgendes.

    :::code language="razor" source="../demo/GraphTutorial/Shared/NavMenu.razor" id="NavMenuSnippet":::

1. Öffnen Sie **./pages/index.Razor** , und ersetzen Sie den Inhalt durch Folgendes.

    :::code language="razor" source="../demo/GraphTutorial/Pages/Index.razor" id="IndexSnippet":::

1. Öffnen Sie **./Shared/LoginDisplay.Razor** , und ersetzen Sie den Inhalt durch Folgendes.

    ```razor
    @using Microsoft.AspNetCore.Components.Authorization
    @using Microsoft.AspNetCore.Components.WebAssembly.Authentication

    @inject NavigationManager Navigation
    @inject SignOutSessionStateManager SignOutManager

    <AuthorizeView>
        <Authorized>
            <a class="text-decoration-none" data-toggle="dropdown" href="#" role="button">
                <img src="/img/no-profile-photo.png" class="nav-profile-photo rounded-circle align-self-center mr-2">
            </a>
            <div class="dropdown-menu dropdown-menu-right">
                <h5 class="dropdown-item-text mb-0">@context.User.Identity.Name</h5>
                <p class="dropdown-item-text text-muted mb-0">placeholder@contoso.com</p>
                <div class="dropdown-divider"></div>
                <button class="dropdown-item" @onclick="BeginLogout">Log out</button>
            </div>
        </Authorized>
        <NotAuthorized>
            <a href="authentication/login">Log in</a>
        </NotAuthorized>
    </AuthorizeView>

    @code{
        private async Task BeginLogout(MouseEventArgs args)
        {
            await SignOutManager.SetSignOutState();
            Navigation.NavigateTo("authentication/logout");
        }
    }
    ```

1. Erstellen Sie ein neues Verzeichnis im **./wwwroot** -Verzeichnis mit dem Namen **IMG**. Fügen Sie in diesem Verzeichnis eine Bilddatei Ihrer Wahl namens **no-profile-photo.png** hinzu. Dieses Bild wird als Foto des Benutzers verwendet, wenn der Benutzer in Microsoft Graph kein Foto hat.

    > [!TIP]
    > Sie können das in diesen Screenshots verwendete Bild von [GitHub](https://github.com/microsoftgraph/msgraph-training-blazor-clientside/blob/master/demo/GraphTutorial/wwwroot/img/no-profile-photo.png)herunterladen.

1. Speichern Sie alle Änderungen, und aktualisieren Sie die Seite.

    ![Screenshot der neu gestalteten Homepage](./images/create-app-01.png)
