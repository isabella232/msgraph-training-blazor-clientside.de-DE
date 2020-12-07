---
ms.openlocfilehash: 33bdb86a4a4af997c8ca522e23ec2f883fdeda88
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584621"
---
<!-- markdownlint-disable MD002 MD041 -->

In diesem Abschnitt werden Sie Microsoft Graph weiter in die Anwendung integrieren, um eine Ansicht des Kalenders des Benutzers für die aktuelle Woche zu erhalten.

## <a name="get-a-calendar-view"></a>Abrufen einer Kalenderansicht

1. Erstellen Sie eine neue Datei im Verzeichnis **./Pages** namens **Calendar. Razor** , und fügen Sie den folgenden Code hinzu.

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

1. Fügen Sie im Abschnitt den folgenden Code hinzu `@code{}` .

    :::code language="csharp" source="../demo/GraphTutorial/Pages/Calendar.razor" id="GetEventsSnippet":::

    Überprüfen Sie die Funktionsweise dieses Codes.

    - Er ruft die Zeitzone des aktuellen Benutzers, das Datumsformat und das Zeitformat aus den benutzerdefinierten Ansprüchen ab, die dem Benutzer hinzugefügt werden.
    - Er berechnet den Anfang und das Ende der aktuellen Woche in der bevorzugten Zeitzone des Benutzers.
    - Es ruft eine Kalenderansicht aus Microsoft Graph für die aktuelle Woche ab.
        - Sie enthält die `Prefer: outlook.timezone` Kopfzeile, damit Microsoft Graph die `start` `end` Eigenschaften und Eigenschaften in der angegebenen Zeitzone zurückgibt.
        - Er verwendet `Top(50)` , um bis zu 50 Ereignisse in der Antwort anzufordern.
        - Sie verwendet `Select(u => new {})` , um nur die von der APP verwendeten Eigenschaften anzufordern.
        - Es verwendet `OrderBy("start/dateTime")` , um die Ergebnisse nach Startzeit zu sortieren.

1. Speichern Sie alle Änderungen, und starten Sie die App neu. Wählen Sie das Navigationselement **Kalender** aus. Die APP zeigt eine JSON-Darstellung der Ereignisse an, die von Microsoft Graph zurückgegeben werden.

## <a name="display-the-results"></a>Anzeigen der Ergebnisse

Nun können Sie das JSON-Dump durch eine benutzerfreundlichere Lösung ersetzen.

1. Fügen Sie im Abschnitt die folgende Funktion hinzu `@code{}` .

    :::code language="csharp" source="../demo/GraphTutorial/Pages/Calendar.razor" id="FormatDateSnippet":::

    Dieser Code verwendet eine ISO 8601-Datumszeichenfolge und wandelt sie in das bevorzugte Datum-und Uhrzeitformat des Benutzers um.

1. Ersetzen Sie das `<code>` -Element innerhalb des `<Authorized>` -Elements durch Folgendes.

    :::code language="razor" source="../demo/GraphTutorial/Pages/Calendar.razor" id="CalendarViewSnippet":::

    Dadurch wird eine Tabelle der Ereignisse erstellt, die von Microsoft Graph zurückgegeben werden.

1. Speichern Sie die Änderungen, und starten Sie die App neu. Auf der **Kalender** Seite wird nun eine Tabelle mit Ereignissen gerendert.

    ![Ein Screenshot der APP mit einer Tabelle mit Ereignissen](images/calendar-view.png)
