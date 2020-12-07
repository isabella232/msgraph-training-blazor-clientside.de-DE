---
ms.openlocfilehash: f98548a1332417d92b126a2cb7499fea5306b34f
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584579"
---
<!-- markdownlint-disable MD002 MD041 -->

In dieser Übung erweitern Sie die Anwendung aus der vorherigen Übung, um die Authentifizierung mit Azure AD zu unterstützen. Dies ist notwendig, um das erforderliche OAuth-Zugriffstoken zum Aufruf der Microsoft Graph-API abzurufen.

1. Öffnen Sie **./wwwroot/appsettings.jsein**. Fügen Sie eine Eigenschaft hinzu, `GraphScopes` und aktualisieren Sie die `Authority` and- `ClientId` Werte so, dass Sie der folgenden entsprechen.

    :::code language="json" source="../demo/GraphTutorial/wwwroot/appsettings.example.json" highlight="3-4,7":::

    Ersetzen `YOUR_APP_ID_HERE` Sie durch die Anwendungs-ID aus Ihrer APP-Registrierung.

    > [!IMPORTANT]
    > Wenn Sie die Quellcodeverwaltung wie git verwenden, wäre es nun ein guter Zeitpunkt, die Datei **appsettings.js** aus der Quellcodeverwaltung auszuschließen, um unbeabsichtigtes Auslaufen ihrer APP-ID zu vermeiden.

    Überprüfen Sie die in dem Wert enthaltenen Bereiche `GraphScopes` .

    - **User. Read** ermöglicht der Anwendung das Abrufen des Profils und des Fotos des Benutzers.
    - **Mailbox Settings. Read** ermöglicht der Anwendung das Abrufen von Postfacheinstellungen, einschließlich der bevorzugten Zeitzone des Benutzers.
    - Mit **Calendars. ReadWrite** kann die Anwendung den Kalender des Benutzers lesen und schreiben.

## <a name="implement-sign-in"></a>Implementieren der Anmeldung

Zu diesem Zeitpunkt hat die .net-Kernprojekt Vorlage den Code hinzugefügt, um die Anmeldung zu aktivieren. In diesem Abschnitt fügen Sie jedoch zusätzlichen Code hinzu, um die Benutzerfreundlichkeit zu verbessern, indem Sie der Identität des Benutzers Informationen aus Microsoft Graph hinzufügen.

1. Öffnen Sie **./Pages/Authentication.Razor** , und ersetzen Sie den Inhalt durch Folgendes.

    :::code language="razor" source="../demo/GraphTutorial/Pages/Authentication.razor" id="AuthenticationSnippet":::

    Dadurch wird die Standardfehlermeldung ersetzt, wenn bei der Anmeldung keine Fehlermeldung angezeigt wird, die vom Authentifizierungsprozess zurückgegeben wird.

1. Erstellen Sie ein neues Verzeichnis im Stamm des Projekts mit dem Namen **Graph**.

1. Erstellen Sie eine neue Datei im **./Graph** -Verzeichnis mit dem Namen **GraphUserAccountFactory.cs** , und fügen Sie den folgenden Code hinzu.

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

    Diese Klasse erweitert die **AccountClaimsPrincipalFactory** -Klasse und überschreibt die `CreateUserAsync` -Methode. Für den Moment protokolliert diese Methode nur das Zugriffstoken zu Debugging-Zwecken. Sie implementieren die Microsoft Graph-Aufrufe später in dieser Übung.

1. Öffnen Sie **./Program.cs** , und fügen Sie die folgenden `using` Anweisungen am Anfang der Datei hinzu.

    ```csharp
    using Microsoft.AspNetCore.Components.WebAssembly.Authentication;
    using GraphTutorial.Graph;
    ```

1. Ersetzen Sie Inside `Main` den vorhandenen `builder.Services.AddMsalAuthentication` Aufruf durch Folgendes.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="AddMsalAuthSnippet":::

    Überprüfen Sie die Funktionsweise dieses Codes.

    - Er lädt den Wert von `GraphScopes` von **appsettings.jsauf** und fügt die einzelnen Bereiche zu den Standardbereichen hinzu, die vom MSAL-Anbieter verwendet werden.
    - Es ersetzt die vorhandene Firma Factory durch die **GraphUserAccountFactory** -Klasse.

1. Speichern Sie die Änderungen, und starten Sie die App neu. Melden Sie sich über den Link **Anmelden** an. Überprüfen und akzeptieren Sie die angeforderten Berechtigungen.

1. Die APP wird mit einer Willkommensnachricht aktualisiert. Greifen Sie auf die Entwicklertools Ihres Browsers zu, und überprüfen Sie die Registerkarte **Konsole** . Die APP protokolliert das Zugriffstoken.

    ![Ein Screenshot des Fensters Browser Developer Tools, in dem das Zugriffstoken angezeigt wird](images/access-token.png)

## <a name="get-user-details"></a>Benutzerdetails abrufen

Sobald sich der Benutzer angemeldet hat, können Sie dessen Informationen über Microsoft Graph abrufen. In diesem Abschnitt verwenden Sie Informationen aus Microsoft Graph, um dem **ClaimsPrincipal** des Benutzers weitere Ansprüche hinzuzufügen.

1. Erstellen Sie eine neue Datei im **./Graph** -Verzeichnis mit dem Namen **GraphClaimsPrincipalExtensions.cs** , und fügen Sie den folgenden Code hinzu.

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphClaimsPrincipalExtensions.cs" id="GraphClaimsExtensionsSnippet":::

    Dieser Code implementiert Erweiterungsmethoden für die **ClaimsPrincipal** -Klasse, mit der Sie Ansprüche mit Werten aus Microsoft Graph-Objekten abrufen und festlegen können.

1. Erstellen Sie eine neue Datei im **./Graph** -Verzeichnis mit dem Namen **BlazorAuthProvider.cs** , und fügen Sie den folgenden Code hinzu.

    :::code language="csharp" source="../demo/GraphTutorial/Graph/BlazorAuthProvider.cs" id="BlazorAuthProviderSnippet":::

    Dieser Code implementiert einen Authentifizierungsanbieter für das Microsoft Graph-SDK, der das vom **Microsoft. AspNetCore. Components. Webassembly. Authentication-** Paket bereitgestellte **IAccessTokenProviderAccessor** verwendet, um Zugriffstoken abzurufen.

1. Erstellen Sie eine neue Datei im **./Graph** -Verzeichnis mit dem Namen **GraphClientFactory.cs** , und fügen Sie den folgenden Code hinzu.

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphClientFactory.cs" id="GraphClientFactorySnippet":::

    Diese Klasse erstellt ein **GraphServiceClient** , das mit **BlazorAuthProvider** konfiguriert ist.

1. Öffnen Sie **./Program.cs** , und ändern Sie die **BaseAddress** der neuen **HttpClient** in `"https://graph.microsoft.com"` .

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="HttpClientSnippet":::

1. Fügen Sie vor der Codezeile den folgenden Code hinzu `await builder.Build().RunAsync();` .

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="AddGraphClientFactorySnippet":::

    Dadurch wird die **GraphClientFactory** als Dienst mit umfangreichem Inhalt hinzugefügt, den wir über Dependency Injection zur Verfügung stellen können.

1. Öffnen Sie **/Graph/GraphUserAccountFactory.cs** , und fügen Sie der Klasse die folgende Eigenschaft hinzu.

    ```csharp
    private readonly GraphClientFactory clientFactory;
    ```

1. Aktualisieren Sie den Konstruktor, um einen **GraphClientFactory** -Parameter zu erstellen und ihn der Eigenschaft zuzuweisen `clientFactory` .

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphUserAccountFactory.cs" id="ConstructorSnippet" highlight="2,7":::

1. Ersetzen Sie die vorhandene `AddGraphInfoToClaims`-Funktion durch Folgendes.

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphUserAccountFactory.cs" id="AddGraphInfoToClaimsSnippet":::

    Überprüfen Sie die Funktionsweise dieses Codes.

    - Es [Ruft das Profil des Benutzers ab](https://docs.microsoft.com/graph/api/user-get).
        - Es wird verwendet `Select` , um zu begrenzen, welche Eigenschaften zurückgegeben werden.
    - Es [Ruft das Foto des Benutzers ab](https://docs.microsoft.com/graph/api/profilephoto-get).
        - Er fordert speziell die 48x48-Pixel Version des Fotos des Benutzers an.
    - Die Informationen werden dem **ClaimsPrincipal** hinzugefügt.

1. Öffnen Sie **/Shared/LoginDisplay.Razor** , und nehmen Sie die folgenden Änderungen vor.

    - Ersetzen Sie `/img/no-profile-photo.png` durch `@(context.User.GetUserGraphPhoto() ?? "/img/no-profile-photo.png")` .
    - Ersetzen Sie `placeholder@contoso.com` durch `@context.User.GetUserGraphEmail()` .

    ```razor
    ...
    <img src="@(context.User.GetUserGraphPhoto() ?? "/img/no-profile-photo.png")"
         class="nav-profile-photo rounded-circle align-self-center mr-2">

    ...

    <p class="dropdown-item-text text-muted mb-0">@context.User.GetUserGraphEmail()</p>
    ...
    ```

1. Speichern Sie alle Änderungen, und starten Sie die App neu. Melden Sie sich bei der APP an. Die APP wird aktualisiert, um das Foto des Benutzers im oberen Menü anzuzeigen. Wenn Sie das Foto des Benutzers auswählen, wird ein Dropdownmenü mit dem Namen des Benutzers, der e-Mail-Adresse und der Schaltfläche " **Abmelden** " geöffnet.

    ![Ein Screenshot der APP mit dem Foto des Benutzers angezeigt](images/user-photo.png)

    ![Screenshot des Dropdownmenüs](images/user-info.png)
