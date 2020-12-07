---
ms.openlocfilehash: 744df064e4fdc1bbf7821ae43a7b7878148902e9
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584616"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="1eae6-101">Erstellen Sie zunächst eine Blazer-Webassembly-app.</span><span class="sxs-lookup"><span data-stu-id="1eae6-101">Start by creating a Blazor WebAssembly app.</span></span>

1. <span data-ttu-id="1eae6-102">Öffnen Sie die Befehlszeilenschnittstelle (CLI) in einem Verzeichnis, in dem Sie das Projekt erstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="1eae6-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="1eae6-103">Führen Sie den folgenden Befehl aus.</span><span class="sxs-lookup"><span data-stu-id="1eae6-103">Run the following command.</span></span>

    ```Shell
    dotnet new blazorwasm --auth SingleOrg -o GraphTutorial
    ```

    <span data-ttu-id="1eae6-104">Der `--auth SingleOrg` Parameter bewirkt, dass das generierte Projekt die Konfiguration für die Authentifizierung mit der Microsoft Identity-Plattform einschließt.</span><span class="sxs-lookup"><span data-stu-id="1eae6-104">The `--auth SingleOrg` parameter causes the generated project to include configuration for authentication with the Microsoft identity platform.</span></span>

1. <span data-ttu-id="1eae6-105">Nachdem das Projekt erstellt wurde, stellen Sie sicher, dass es funktioniert, indem Sie das aktuelle Verzeichnis in das **GraphTutorial** -Verzeichnis ändern und den folgenden Befehl in ihrer CLI ausführen.</span><span class="sxs-lookup"><span data-stu-id="1eae6-105">Once the project is created, verify that it works by changing the current directory to the **GraphTutorial** directory and running the following command in your CLI.</span></span>

    ```Shell
    dotnet watch run
    ```

1. <span data-ttu-id="1eae6-106">Öffnen Sie den Browser, und navigieren Sie zu `https://localhost:5001` .</span><span class="sxs-lookup"><span data-stu-id="1eae6-106">Open your browser and browse to `https://localhost:5001`.</span></span> <span data-ttu-id="1eae6-107">Wenn alles funktioniert, sollte ein "Hello, World!" angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="1eae6-107">If everything is working, you should see a "Hello, world!"</span></span> <span data-ttu-id="1eae6-108">Nachricht.</span><span class="sxs-lookup"><span data-stu-id="1eae6-108">message.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1eae6-109">Wenn Sie eine Warnung erhalten, dass das Zertifikat für **localhost** nicht vertrauenswürdig ist, können Sie die .net-Kern-CLI verwenden, um das Entwicklungszertifikat zu installieren und ihm zu vertrauen.</span><span class="sxs-lookup"><span data-stu-id="1eae6-109">If you receive a warning that the certificate for **localhost** is un-trusted you can use the .NET Core CLI to install and trust the development certificate.</span></span> <span data-ttu-id="1eae6-110">Anweisungen zu bestimmten Betriebssystemen finden Sie unter [Erzwingen von HTTPS in ASP.net Core](/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.1) .</span><span class="sxs-lookup"><span data-stu-id="1eae6-110">See [Enforce HTTPS in ASP.NET Core](/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.1) for instructions for specific operating systems.</span></span>

## <a name="add-nuget-packages"></a><span data-ttu-id="1eae6-111">Hinzufügen von NuGet-Paketen</span><span class="sxs-lookup"><span data-stu-id="1eae6-111">Add NuGet packages</span></span>

<span data-ttu-id="1eae6-112">Bevor Sie fortfahren, installieren Sie einige zusätzliche NuGet-Pakete, die Sie später verwenden werden.</span><span class="sxs-lookup"><span data-stu-id="1eae6-112">Before moving on, install some additional NuGet packages that you will use later.</span></span>

- <span data-ttu-id="1eae6-113">[Microsoft.Graph](https://www.nuget.org/packages/Microsoft.Graph/) zum Aufrufen von Microsoft Graph</span><span class="sxs-lookup"><span data-stu-id="1eae6-113">[Microsoft.Graph](https://www.nuget.org/packages/Microsoft.Graph/) for making calls to Microsoft Graph.</span></span>
- <span data-ttu-id="1eae6-114">[TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) für die Übersetzung von Windows-Zeitzonenbezeichnern in IANA-IDs.</span><span class="sxs-lookup"><span data-stu-id="1eae6-114">[TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) for translating Windows time zone identifiers to IANA identifiers.</span></span>

1. <span data-ttu-id="1eae6-115">Führen Sie die folgenden Befehle in der CLI aus, um die Abhängigkeiten zu installieren.</span><span class="sxs-lookup"><span data-stu-id="1eae6-115">Run the following commands in your CLI to install the dependencies.</span></span>

    ```Shell
    dotnet add package Microsoft.Graph --version 3.18.0
    dotnet add package TimeZoneConverter
    ```

## <a name="design-the-app"></a><span data-ttu-id="1eae6-116">Entwerfen der App</span><span class="sxs-lookup"><span data-stu-id="1eae6-116">Design the app</span></span>

<span data-ttu-id="1eae6-117">In diesem Abschnitt wird die grundlegende UI-Struktur der Anwendung erstellt.</span><span class="sxs-lookup"><span data-stu-id="1eae6-117">In this section you will create the basic UI structure of the application.</span></span>

1. <span data-ttu-id="1eae6-118">Entfernen von Beispielseiten, die von der Vorlage generiert wurden.</span><span class="sxs-lookup"><span data-stu-id="1eae6-118">Remove sample pages generated by the template.</span></span> <span data-ttu-id="1eae6-119">Löschen Sie die folgenden Dateien.</span><span class="sxs-lookup"><span data-stu-id="1eae6-119">Delete the following files.</span></span>

    - <span data-ttu-id="1eae6-120">**./Pages/Counter.razor**</span><span class="sxs-lookup"><span data-stu-id="1eae6-120">**./Pages/Counter.razor**</span></span>
    - <span data-ttu-id="1eae6-121">**./Pages/FetchData.razor**</span><span class="sxs-lookup"><span data-stu-id="1eae6-121">**./Pages/FetchData.razor**</span></span>
    - <span data-ttu-id="1eae6-122">**./Shared/SurveyPrompt.razor**</span><span class="sxs-lookup"><span data-stu-id="1eae6-122">**./Shared/SurveyPrompt.razor**</span></span>
    - <span data-ttu-id="1eae6-123">**./wwwroot/Sample-Data/weather.jsein**</span><span class="sxs-lookup"><span data-stu-id="1eae6-123">**./wwwroot/sample-data/weather.json**</span></span>

1. <span data-ttu-id="1eae6-124">Öffnen Sie **./wwwroot/index.html** , und fügen Sie den folgenden Code direkt **vor** dem Endtag hinzu `</body>` .</span><span class="sxs-lookup"><span data-stu-id="1eae6-124">Open **./wwwroot/index.html** and add the following code just **before** the closing `</body>` tag.</span></span>

    :::code language="html" source="../demo/GraphTutorial/wwwroot/index.html" id="BootStrapJSSnippet":::

    <span data-ttu-id="1eae6-125">Dadurch werden die [Bootstrap](https://getbootstrap.com/docs/4.5/getting-started/introduction/) -JavaScript-Dateien hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="1eae6-125">This adds the [Bootstrap](https://getbootstrap.com/docs/4.5/getting-started/introduction/) javascript files.</span></span>

1. <span data-ttu-id="1eae6-126">Öffnen Sie **/wwwroot/CSS/app.CSS** , und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="1eae6-126">Open **./wwwroot/css/app.css** and add the following code.</span></span>

    :::code language="css" source="../demo/GraphTutorial/wwwroot/css/app.css" id="CssSnippet":::

1. <span data-ttu-id="1eae6-127">Öffnen Sie **./Shared/NavMenu.Razor** , und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="1eae6-127">Open **./Shared/NavMenu.razor** and replace its contents with the following.</span></span>

    :::code language="razor" source="../demo/GraphTutorial/Shared/NavMenu.razor" id="NavMenuSnippet":::

1. <span data-ttu-id="1eae6-128">Öffnen Sie **./pages/index.Razor** , und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="1eae6-128">Open **./Pages/Index.razor** and replace its contents with the following.</span></span>

    :::code language="razor" source="../demo/GraphTutorial/Pages/Index.razor" id="IndexSnippet":::

1. <span data-ttu-id="1eae6-129">Öffnen Sie **./Shared/LoginDisplay.Razor** , und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="1eae6-129">Open **./Shared/LoginDisplay.razor** and replace its contents with the following.</span></span>

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

1. <span data-ttu-id="1eae6-130">Erstellen Sie ein neues Verzeichnis im **./wwwroot** -Verzeichnis mit dem Namen **IMG**.</span><span class="sxs-lookup"><span data-stu-id="1eae6-130">Create a new directory in the **./wwwroot** directory named **img**.</span></span> <span data-ttu-id="1eae6-131">Fügen Sie in diesem Verzeichnis eine Bilddatei Ihrer Wahl namens **no-profile-photo.png** hinzu.</span><span class="sxs-lookup"><span data-stu-id="1eae6-131">Add an image file of your choosing named **no-profile-photo.png** in this directory.</span></span> <span data-ttu-id="1eae6-132">Dieses Bild wird als Foto des Benutzers verwendet, wenn der Benutzer in Microsoft Graph kein Foto hat.</span><span class="sxs-lookup"><span data-stu-id="1eae6-132">This image will be used as the user's photo when the user has no photo in Microsoft Graph.</span></span>

    > [!TIP]
    > <span data-ttu-id="1eae6-133">Sie können das in diesen Screenshots verwendete Bild von [GitHub](https://github.com/microsoftgraph/msgraph-training-blazor-clientside/blob/master/demo/GraphTutorial/wwwroot/img/no-profile-photo.png)herunterladen.</span><span class="sxs-lookup"><span data-stu-id="1eae6-133">You can download the image used in these screenshots from [GitHub](https://github.com/microsoftgraph/msgraph-training-blazor-clientside/blob/master/demo/GraphTutorial/wwwroot/img/no-profile-photo.png).</span></span>

1. <span data-ttu-id="1eae6-134">Speichern Sie alle Änderungen, und aktualisieren Sie die Seite.</span><span class="sxs-lookup"><span data-stu-id="1eae6-134">Save all of your changes and refresh the page.</span></span>

    ![Screenshot der neu gestalteten Homepage](./images/create-app-01.png)
