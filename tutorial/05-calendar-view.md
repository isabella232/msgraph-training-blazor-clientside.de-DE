---
ms.openlocfilehash: 33bdb86a4a4af997c8ca522e23ec2f883fdeda88
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584621"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="33612-101">In diesem Abschnitt werden Sie Microsoft Graph weiter in die Anwendung integrieren, um eine Ansicht des Kalenders des Benutzers für die aktuelle Woche zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="33612-101">In this section you will further incorporate Microsoft Graph into the application to get a view of the user's calendar for the current week.</span></span>

## <a name="get-a-calendar-view"></a><span data-ttu-id="33612-102">Abrufen einer Kalenderansicht</span><span class="sxs-lookup"><span data-stu-id="33612-102">Get a calendar view</span></span>

1. <span data-ttu-id="33612-103">Erstellen Sie eine neue Datei im Verzeichnis **./Pages** namens **Calendar. Razor** , und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="33612-103">Create a new file in the **./Pages** directory named **Calendar.razor** and add the following code.</span></span>

    ```razor
    @page "/calendar"
    @using Microsoft.Graph
    @using TimeZoneConverter

    @inject GraphTutorial.Graph.GraphClientFactory clientFactory

    <AuthorizeView>
        <Authorized>
            <!-- Temporary JSON dump of events -->
            <code>@graphClient.HttpProvider.Serializer.SerializeObject(events)</code>
        </Authorized>
        <NotAuthorized>
            <RedirectToLogin />
        </NotAuthorized>
    </AuthorizeView>

    @code{
        [CascadingParameter]
        private Task<AuthenticationState> authenticationStateTask { get; set; }

        private GraphServiceClient graphClient;
        private IList<Event> events = new List<Event>();
        private string dateTimeFormat;
    }
    ```

1. <span data-ttu-id="33612-104">Fügen Sie im Abschnitt den folgenden Code hinzu `@code{}` .</span><span class="sxs-lookup"><span data-stu-id="33612-104">Add the following code inside the `@code{}` section.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Pages/Calendar.razor" id="GetEventsSnippet":::

    <span data-ttu-id="33612-105">Überprüfen Sie die Funktionsweise dieses Codes.</span><span class="sxs-lookup"><span data-stu-id="33612-105">Consider what this code does.</span></span>

    - <span data-ttu-id="33612-106">Er ruft die Zeitzone des aktuellen Benutzers, das Datumsformat und das Zeitformat aus den benutzerdefinierten Ansprüchen ab, die dem Benutzer hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="33612-106">It gets the current user's time zone, date format, and time format from the custom claims added to the user.</span></span>
    - <span data-ttu-id="33612-107">Er berechnet den Anfang und das Ende der aktuellen Woche in der bevorzugten Zeitzone des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="33612-107">It calculates the start and end of the current week in the user's preferred time zone.</span></span>
    - <span data-ttu-id="33612-108">Es ruft eine Kalenderansicht aus Microsoft Graph für die aktuelle Woche ab.</span><span class="sxs-lookup"><span data-stu-id="33612-108">It gets a calendar view from Microsoft Graph for the current week.</span></span>
        - <span data-ttu-id="33612-109">Sie enthält die `Prefer: outlook.timezone` Kopfzeile, damit Microsoft Graph die `start` `end` Eigenschaften und Eigenschaften in der angegebenen Zeitzone zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="33612-109">It includes the `Prefer: outlook.timezone` header to cause Microsoft Graph to return the `start` and `end` properties in the specified time zone.</span></span>
        - <span data-ttu-id="33612-110">Er verwendet `Top(50)` , um bis zu 50 Ereignisse in der Antwort anzufordern.</span><span class="sxs-lookup"><span data-stu-id="33612-110">It uses `Top(50)` to request up to 50 events in the response.</span></span>
        - <span data-ttu-id="33612-111">Sie verwendet `Select(u => new {})` , um nur die von der APP verwendeten Eigenschaften anzufordern.</span><span class="sxs-lookup"><span data-stu-id="33612-111">It uses `Select(u => new {})` to request just the properties used by the app.</span></span>
        - <span data-ttu-id="33612-112">Es verwendet `OrderBy("start/dateTime")` , um die Ergebnisse nach Startzeit zu sortieren.</span><span class="sxs-lookup"><span data-stu-id="33612-112">It uses `OrderBy("start/dateTime")` to sort the results by start time.</span></span>

1. <span data-ttu-id="33612-113">Speichern Sie alle Änderungen, und starten Sie die App neu.</span><span class="sxs-lookup"><span data-stu-id="33612-113">Save all of your changes and restart the app.</span></span> <span data-ttu-id="33612-114">Wählen Sie das Navigationselement **Kalender** aus.</span><span class="sxs-lookup"><span data-stu-id="33612-114">Choose the **Calendar** nav item.</span></span> <span data-ttu-id="33612-115">Die APP zeigt eine JSON-Darstellung der Ereignisse an, die von Microsoft Graph zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="33612-115">The app displays a JSON representation of the events returned from Microsoft Graph.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="33612-116">Anzeigen der Ergebnisse</span><span class="sxs-lookup"><span data-stu-id="33612-116">Display the results</span></span>

<span data-ttu-id="33612-117">Nun können Sie das JSON-Dump durch eine benutzerfreundlichere Lösung ersetzen.</span><span class="sxs-lookup"><span data-stu-id="33612-117">Now you can replace the JSON dump with something more user-friendly.</span></span>

1. <span data-ttu-id="33612-118">Fügen Sie im Abschnitt die folgende Funktion hinzu `@code{}` .</span><span class="sxs-lookup"><span data-stu-id="33612-118">Add the following function inside the `@code{}` section.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Pages/Calendar.razor" id="FormatDateSnippet":::

    <span data-ttu-id="33612-119">Dieser Code verwendet eine ISO 8601-Datumszeichenfolge und wandelt sie in das bevorzugte Datum-und Uhrzeitformat des Benutzers um.</span><span class="sxs-lookup"><span data-stu-id="33612-119">This code takes an ISO 8601 date string and converts it into the user's preferred date and time format.</span></span>

1. <span data-ttu-id="33612-120">Ersetzen Sie das `<code>` -Element innerhalb des `<Authorized>` -Elements durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="33612-120">Replace the `<code>` element inside the `<Authorized>` element with the following.</span></span>

    :::code language="razor" source="../demo/GraphTutorial/Pages/Calendar.razor" id="CalendarViewSnippet":::

    <span data-ttu-id="33612-121">Dadurch wird eine Tabelle der Ereignisse erstellt, die von Microsoft Graph zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="33612-121">This creates a table of the events returned by Microsoft Graph.</span></span>

1. <span data-ttu-id="33612-122">Speichern Sie die Änderungen, und starten Sie die App neu.</span><span class="sxs-lookup"><span data-stu-id="33612-122">Save your changes and restart the app.</span></span> <span data-ttu-id="33612-123">Auf der **Kalender** Seite wird nun eine Tabelle mit Ereignissen gerendert.</span><span class="sxs-lookup"><span data-stu-id="33612-123">Now the **Calendar** page renders a table of events.</span></span>

    ![Ein Screenshot der APP mit einer Tabelle mit Ereignissen](images/calendar-view.png)
