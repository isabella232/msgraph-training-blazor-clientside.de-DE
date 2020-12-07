---
ms.openlocfilehash: f98548a1332417d92b126a2cb7499fea5306b34f
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584579"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="16757-101">In dieser Übung erweitern Sie die Anwendung aus der vorherigen Übung, um die Authentifizierung mit Azure AD zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="16757-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="16757-102">Dies ist notwendig, um das erforderliche OAuth-Zugriffstoken zum Aufruf der Microsoft Graph-API abzurufen.</span><span class="sxs-lookup"><span data-stu-id="16757-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph API.</span></span>

1. <span data-ttu-id="16757-103">Öffnen Sie **./wwwroot/appsettings.jsein**.</span><span class="sxs-lookup"><span data-stu-id="16757-103">Open **./wwwroot/appsettings.json**.</span></span> <span data-ttu-id="16757-104">Fügen Sie eine Eigenschaft hinzu, `GraphScopes` und aktualisieren Sie die `Authority` and- `ClientId` Werte so, dass Sie der folgenden entsprechen.</span><span class="sxs-lookup"><span data-stu-id="16757-104">Add a `GraphScopes` property and update the `Authority` and `ClientId` values to match the following.</span></span>

    :::code language="json" source="../demo/GraphTutorial/wwwroot/appsettings.example.json" highlight="3-4,7":::

    <span data-ttu-id="16757-105">Ersetzen `YOUR_APP_ID_HERE` Sie durch die Anwendungs-ID aus Ihrer APP-Registrierung.</span><span class="sxs-lookup"><span data-stu-id="16757-105">Replace `YOUR_APP_ID_HERE` with the application ID from your app registration.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="16757-106">Wenn Sie die Quellcodeverwaltung wie git verwenden, wäre es nun ein guter Zeitpunkt, die Datei **appsettings.js** aus der Quellcodeverwaltung auszuschließen, um unbeabsichtigtes Auslaufen ihrer APP-ID zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="16757-106">If you're using source control such as git, now would be a good time to exclude the **appsettings.json** file from source control to avoid inadvertently leaking your app ID.</span></span>

    <span data-ttu-id="16757-107">Überprüfen Sie die in dem Wert enthaltenen Bereiche `GraphScopes` .</span><span class="sxs-lookup"><span data-stu-id="16757-107">Review the scopes included in the `GraphScopes` value.</span></span>

    - <span data-ttu-id="16757-108">**User. Read** ermöglicht der Anwendung das Abrufen des Profils und des Fotos des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="16757-108">**User.Read** allows the application to get the user's profile and photo.</span></span>
    - <span data-ttu-id="16757-109">**Mailbox Settings. Read** ermöglicht der Anwendung das Abrufen von Postfacheinstellungen, einschließlich der bevorzugten Zeitzone des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="16757-109">**MailboxSettings.Read** allows the application to get mailbox settings, which includes the user's preferred time zone.</span></span>
    - <span data-ttu-id="16757-110">Mit **Calendars. ReadWrite** kann die Anwendung den Kalender des Benutzers lesen und schreiben.</span><span class="sxs-lookup"><span data-stu-id="16757-110">**Calendars.ReadWrite** allows the application to read and write to the user's calendar.</span></span>

## <a name="implement-sign-in"></a><span data-ttu-id="16757-111">Implementieren der Anmeldung</span><span class="sxs-lookup"><span data-stu-id="16757-111">Implement sign-in</span></span>

<span data-ttu-id="16757-112">Zu diesem Zeitpunkt hat die .net-Kernprojekt Vorlage den Code hinzugefügt, um die Anmeldung zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="16757-112">At this point the .NET Core project template has added the code to enable sign in.</span></span> <span data-ttu-id="16757-113">In diesem Abschnitt fügen Sie jedoch zusätzlichen Code hinzu, um die Benutzerfreundlichkeit zu verbessern, indem Sie der Identität des Benutzers Informationen aus Microsoft Graph hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="16757-113">However, in this section you'll add additional code to improve the experience by adding information from Microsoft Graph to the user's identity.</span></span>

1. <span data-ttu-id="16757-114">Öffnen Sie **./Pages/Authentication.Razor** , und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="16757-114">Open **./Pages/Authentication.razor** and replace its contents with the following.</span></span>

    :::code language="razor" source="../demo/GraphTutorial/Pages/Authentication.razor" id="AuthenticationSnippet":::

    <span data-ttu-id="16757-115">Dadurch wird die Standardfehlermeldung ersetzt, wenn bei der Anmeldung keine Fehlermeldung angezeigt wird, die vom Authentifizierungsprozess zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="16757-115">This replaces the default error message when login fails to display any error message returned by the authentication process.</span></span>

1. <span data-ttu-id="16757-116">Erstellen Sie ein neues Verzeichnis im Stamm des Projekts mit dem Namen **Graph**.</span><span class="sxs-lookup"><span data-stu-id="16757-116">Create a new directory in the root of the project named **Graph**.</span></span>

1. <span data-ttu-id="16757-117">Erstellen Sie eine neue Datei im **./Graph** -Verzeichnis mit dem Namen **GraphUserAccountFactory.cs** , und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="16757-117">Create a new file in the **./Graph** directory named **GraphUserAccountFactory.cs** and add the following code.</span></span>

    ```csharp
    using System.Security.Claims;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Components.WebAssembly.Authentication;
    using Microsoft.AspNetCore.Components.WebAssembly.Authentication.Internal;
    using Microsoft.Extensions.Logging;
    using Microsoft.Graph;

    namespace GraphTutorial.Graph
    {
        // Extends the AccountClaimsPrincipalFactory that builds
        // a user identity from the identity token.
        // This class adds additional claims to the user's ClaimPrincipal
        // that hold values from Microsoft Graph
        public class GraphUserAccountFactory
            : AccountClaimsPrincipalFactory<RemoteUserAccount>
        {
            private readonly IAccessTokenProviderAccessor accessor;
            private readonly ILogger<GraphUserAccountFactory> logger;

            public GraphUserAccountFactory(IAccessTokenProviderAccessor accessor,
                ILogger<GraphUserAccountFactory> logger)
            : base(accessor)
            {
                this.accessor = accessor;
                this.logger = logger;
            }

            public async override ValueTask<ClaimsPrincipal> CreateUserAsync(
                RemoteUserAccount account,
                RemoteAuthenticationUserOptions options)
            {
                // Create the base user
                var initialUser = await base.CreateUserAsync(account, options);

                // If authenticated, we can call Microsoft Graph
                if (initialUser.Identity.IsAuthenticated)
                {
                    try
                    {
                        // Add additional info from Graph to the identity
                        await AddGraphInfoToClaims(accessor, initialUser);
                    }
                    catch (AccessTokenNotAvailableException exception)
                    {
                        logger.LogError($"Graph API access token failure: {exception.Message}");
                    }
                    catch (ServiceException exception)
                    {
                        logger.LogError($"Graph API error: {exception.Message}");
                        logger.LogError($"Response body: {exception.RawResponseBody}");
                    }
                }

                return initialUser;
            }

            private async Task AddGraphInfoToClaims(
                IAccessTokenProviderAccessor accessor,
                ClaimsPrincipal claimsPrincipal)
            {
                // TEMPORARY: Get the token and log it
                var result = await accessor.TokenProvider.RequestAccessToken();

                if (result.TryGetToken(out var token))
                {
                    logger.LogInformation($"Access token: {token.Value}");
                }
            }
        }
    }
    ```

    <span data-ttu-id="16757-118">Diese Klasse erweitert die **AccountClaimsPrincipalFactory** -Klasse und überschreibt die `CreateUserAsync` -Methode.</span><span class="sxs-lookup"><span data-stu-id="16757-118">This class extends the **AccountClaimsPrincipalFactory** class and overrides the `CreateUserAsync` method.</span></span> <span data-ttu-id="16757-119">Für den Moment protokolliert diese Methode nur das Zugriffstoken zu Debugging-Zwecken.</span><span class="sxs-lookup"><span data-stu-id="16757-119">For now, this method only logs the access token for debugging purposes.</span></span> <span data-ttu-id="16757-120">Sie implementieren die Microsoft Graph-Aufrufe später in dieser Übung.</span><span class="sxs-lookup"><span data-stu-id="16757-120">You'll implement the Microsoft Graph calls later in this exercise.</span></span>

1. <span data-ttu-id="16757-121">Öffnen Sie **./Program.cs** , und fügen Sie die folgenden `using` Anweisungen am Anfang der Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="16757-121">Open **./Program.cs** and add the following `using` statements at the top of the file.</span></span>

    ```csharp
    using Microsoft.AspNetCore.Components.WebAssembly.Authentication;
    using GraphTutorial.Graph;
    ```

1. <span data-ttu-id="16757-122">Ersetzen Sie Inside `Main` den vorhandenen `builder.Services.AddMsalAuthentication` Aufruf durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="16757-122">Inside `Main`, replace the existing `builder.Services.AddMsalAuthentication` call with the following.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="AddMsalAuthSnippet":::

    <span data-ttu-id="16757-123">Überprüfen Sie die Funktionsweise dieses Codes.</span><span class="sxs-lookup"><span data-stu-id="16757-123">Consider what this code does.</span></span>

    - <span data-ttu-id="16757-124">Er lädt den Wert von `GraphScopes` von **appsettings.jsauf** und fügt die einzelnen Bereiche zu den Standardbereichen hinzu, die vom MSAL-Anbieter verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="16757-124">It loads the value of `GraphScopes` from **appsettings.json** and adds each scope to the default scopes used by the MSAL provider.</span></span>
    - <span data-ttu-id="16757-125">Es ersetzt die vorhandene Firma Factory durch die **GraphUserAccountFactory** -Klasse.</span><span class="sxs-lookup"><span data-stu-id="16757-125">It replaces the existing account factory with the **GraphUserAccountFactory** class.</span></span>

1. <span data-ttu-id="16757-126">Speichern Sie die Änderungen, und starten Sie die App neu.</span><span class="sxs-lookup"><span data-stu-id="16757-126">Save your changes and restart the app.</span></span> <span data-ttu-id="16757-127">Melden Sie sich über den Link **Anmelden** an.</span><span class="sxs-lookup"><span data-stu-id="16757-127">Use the **Log in** link to log in.</span></span> <span data-ttu-id="16757-128">Überprüfen und akzeptieren Sie die angeforderten Berechtigungen.</span><span class="sxs-lookup"><span data-stu-id="16757-128">Review and accept the requested permissions.</span></span>

1. <span data-ttu-id="16757-129">Die APP wird mit einer Willkommensnachricht aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="16757-129">The app refreshes with a welcome message.</span></span> <span data-ttu-id="16757-130">Greifen Sie auf die Entwicklertools Ihres Browsers zu, und überprüfen Sie die Registerkarte **Konsole** . Die APP protokolliert das Zugriffstoken.</span><span class="sxs-lookup"><span data-stu-id="16757-130">Access your browser's developer tools and review the **Console** tab. The app logs the access token.</span></span>

    ![Ein Screenshot des Fensters Browser Developer Tools, in dem das Zugriffstoken angezeigt wird](images/access-token.png)

## <a name="get-user-details"></a><span data-ttu-id="16757-132">Benutzerdetails abrufen</span><span class="sxs-lookup"><span data-stu-id="16757-132">Get user details</span></span>

<span data-ttu-id="16757-133">Sobald sich der Benutzer angemeldet hat, können Sie dessen Informationen über Microsoft Graph abrufen.</span><span class="sxs-lookup"><span data-stu-id="16757-133">Once the user is logged in, you can get their information from Microsoft Graph.</span></span> <span data-ttu-id="16757-134">In diesem Abschnitt verwenden Sie Informationen aus Microsoft Graph, um dem **ClaimsPrincipal** des Benutzers weitere Ansprüche hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="16757-134">In this section you'll use information from Microsoft Graph to add additional claims to the user's **ClaimsPrincipal**.</span></span>

1. <span data-ttu-id="16757-135">Erstellen Sie eine neue Datei im **./Graph** -Verzeichnis mit dem Namen **GraphClaimsPrincipalExtensions.cs** , und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="16757-135">Create a new file in the **./Graph** directory named **GraphClaimsPrincipalExtensions.cs** and add the following code.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphClaimsPrincipalExtensions.cs" id="GraphClaimsExtensionsSnippet":::

    <span data-ttu-id="16757-136">Dieser Code implementiert Erweiterungsmethoden für die **ClaimsPrincipal** -Klasse, mit der Sie Ansprüche mit Werten aus Microsoft Graph-Objekten abrufen und festlegen können.</span><span class="sxs-lookup"><span data-stu-id="16757-136">This code implements extension methods for the **ClaimsPrincipal** class that allow you to get and set claims with values from Microsoft Graph objects.</span></span>

1. <span data-ttu-id="16757-137">Erstellen Sie eine neue Datei im **./Graph** -Verzeichnis mit dem Namen **BlazorAuthProvider.cs** , und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="16757-137">Create a new file in the **./Graph** directory named **BlazorAuthProvider.cs** and add the following code.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/BlazorAuthProvider.cs" id="BlazorAuthProviderSnippet":::

    <span data-ttu-id="16757-138">Dieser Code implementiert einen Authentifizierungsanbieter für das Microsoft Graph-SDK, der das vom **Microsoft. AspNetCore. Components. Webassembly. Authentication-** Paket bereitgestellte **IAccessTokenProviderAccessor** verwendet, um Zugriffstoken abzurufen.</span><span class="sxs-lookup"><span data-stu-id="16757-138">This code implements an authentication provider for the Microsoft Graph SDK that uses the **IAccessTokenProviderAccessor** provided by the **Microsoft.AspNetCore.Components.WebAssembly.Authentication** package to get access tokens.</span></span>

1. <span data-ttu-id="16757-139">Erstellen Sie eine neue Datei im **./Graph** -Verzeichnis mit dem Namen **GraphClientFactory.cs** , und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="16757-139">Create a new file in the **./Graph** directory named **GraphClientFactory.cs** and add the following code.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphClientFactory.cs" id="GraphClientFactorySnippet":::

    <span data-ttu-id="16757-140">Diese Klasse erstellt ein **GraphServiceClient** , das mit **BlazorAuthProvider** konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="16757-140">This class creates a **GraphServiceClient** configured with the **BlazorAuthProvider**.</span></span>

1. <span data-ttu-id="16757-141">Öffnen Sie **./Program.cs** , und ändern Sie die **BaseAddress** der neuen **HttpClient** in `"https://graph.microsoft.com"` .</span><span class="sxs-lookup"><span data-stu-id="16757-141">Open **./Program.cs** and change the **BaseAddress** of the new **HttpClient** to `"https://graph.microsoft.com"`.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="HttpClientSnippet":::

1. <span data-ttu-id="16757-142">Fügen Sie vor der Codezeile den folgenden Code hinzu `await builder.Build().RunAsync();` .</span><span class="sxs-lookup"><span data-stu-id="16757-142">Add the following code before the `await builder.Build().RunAsync();` line.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="AddGraphClientFactorySnippet":::

    <span data-ttu-id="16757-143">Dadurch wird die **GraphClientFactory** als Dienst mit umfangreichem Inhalt hinzugefügt, den wir über Dependency Injection zur Verfügung stellen können.</span><span class="sxs-lookup"><span data-stu-id="16757-143">This adds the **GraphClientFactory** as a scoped service that we can make available via dependency injection.</span></span>

1. <span data-ttu-id="16757-144">Öffnen Sie **/Graph/GraphUserAccountFactory.cs** , und fügen Sie der Klasse die folgende Eigenschaft hinzu.</span><span class="sxs-lookup"><span data-stu-id="16757-144">Open **./Graph/GraphUserAccountFactory.cs** and add the following property to the class.</span></span>

    ```csharp
    private readonly GraphClientFactory clientFactory;
    ```

1. <span data-ttu-id="16757-145">Aktualisieren Sie den Konstruktor, um einen **GraphClientFactory** -Parameter zu erstellen und ihn der Eigenschaft zuzuweisen `clientFactory` .</span><span class="sxs-lookup"><span data-stu-id="16757-145">Update the constructor to take a **GraphClientFactory** parameter and assign it to the `clientFactory` property.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphUserAccountFactory.cs" id="ConstructorSnippet" highlight="2,7":::

1. <span data-ttu-id="16757-146">Ersetzen Sie die vorhandene `AddGraphInfoToClaims`-Funktion durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="16757-146">Replace the existing `AddGraphInfoToClaims` function with the following.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphUserAccountFactory.cs" id="AddGraphInfoToClaimsSnippet":::

    <span data-ttu-id="16757-147">Überprüfen Sie die Funktionsweise dieses Codes.</span><span class="sxs-lookup"><span data-stu-id="16757-147">Consider what this code does.</span></span>

    - <span data-ttu-id="16757-148">Es [Ruft das Profil des Benutzers ab](https://docs.microsoft.com/graph/api/user-get).</span><span class="sxs-lookup"><span data-stu-id="16757-148">It [gets the user's profile](https://docs.microsoft.com/graph/api/user-get).</span></span>
        - <span data-ttu-id="16757-149">Es wird verwendet `Select` , um zu begrenzen, welche Eigenschaften zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="16757-149">It uses `Select` to limit which properties are returned.</span></span>
    - <span data-ttu-id="16757-150">Es [Ruft das Foto des Benutzers ab](https://docs.microsoft.com/graph/api/profilephoto-get).</span><span class="sxs-lookup"><span data-stu-id="16757-150">It [gets the user's photo](https://docs.microsoft.com/graph/api/profilephoto-get).</span></span>
        - <span data-ttu-id="16757-151">Er fordert speziell die 48x48-Pixel Version des Fotos des Benutzers an.</span><span class="sxs-lookup"><span data-stu-id="16757-151">It requests specifically the 48x48 pixel version of the user's photo.</span></span>
    - <span data-ttu-id="16757-152">Die Informationen werden dem **ClaimsPrincipal** hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="16757-152">It adds the information to the **ClaimsPrincipal**.</span></span>

1. <span data-ttu-id="16757-153">Öffnen Sie **/Shared/LoginDisplay.Razor** , und nehmen Sie die folgenden Änderungen vor.</span><span class="sxs-lookup"><span data-stu-id="16757-153">Open **./Shared/LoginDisplay.razor** and make the following changes.</span></span>

    - <span data-ttu-id="16757-154">Ersetzen Sie `/img/no-profile-photo.png` durch `@(context.User.GetUserGraphPhoto() ?? "/img/no-profile-photo.png")` .</span><span class="sxs-lookup"><span data-stu-id="16757-154">Replace `/img/no-profile-photo.png` with `@(context.User.GetUserGraphPhoto() ?? "/img/no-profile-photo.png")`.</span></span>
    - <span data-ttu-id="16757-155">Ersetzen Sie `placeholder@contoso.com` durch `@context.User.GetUserGraphEmail()` .</span><span class="sxs-lookup"><span data-stu-id="16757-155">Replace `placeholder@contoso.com` with `@context.User.GetUserGraphEmail()`.</span></span>

    ```razor
    ...
    <img src="@(context.User.GetUserGraphPhoto() ?? "/img/no-profile-photo.png")"
         class="nav-profile-photo rounded-circle align-self-center mr-2">

    ...

    <p class="dropdown-item-text text-muted mb-0">@context.User.GetUserGraphEmail()</p>
    ...
    ```

1. <span data-ttu-id="16757-156">Speichern Sie alle Änderungen, und starten Sie die App neu.</span><span class="sxs-lookup"><span data-stu-id="16757-156">Save all of your changes and restart the app.</span></span> <span data-ttu-id="16757-157">Melden Sie sich bei der APP an.</span><span class="sxs-lookup"><span data-stu-id="16757-157">Log into the app.</span></span> <span data-ttu-id="16757-158">Die APP wird aktualisiert, um das Foto des Benutzers im oberen Menü anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="16757-158">The app updates to show the user's photo in the top menu.</span></span> <span data-ttu-id="16757-159">Wenn Sie das Foto des Benutzers auswählen, wird ein Dropdownmenü mit dem Namen des Benutzers, der e-Mail-Adresse und der Schaltfläche " **Abmelden** " geöffnet.</span><span class="sxs-lookup"><span data-stu-id="16757-159">Selecting the user's photo opens a drop-down menu with the user's name, email address, and a **Log out** button.</span></span>

    ![Ein Screenshot der APP mit dem Foto des Benutzers angezeigt](images/user-photo.png)

    ![Screenshot des Dropdownmenüs](images/user-info.png)
