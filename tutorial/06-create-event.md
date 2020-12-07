---
ms.openlocfilehash: 723f1d08b9d1085d47d0d5fac71da5371badc3d0
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584574"
---
<!-- markdownlint-disable MD002 MD041 -->

In diesem Abschnitt können Sie die Möglichkeit zum Erstellen von Ereignissen im Kalender des Benutzers hinzufügen.

1. Erstellen Sie eine neue Datei im **./Pages** -Verzeichnis mit dem Namen "New **Event. Razor** ", und fügen Sie den folgenden Code hinzu.

    :::code language="razor" source="../demo/GraphTutorial/Pages/NewEvent.razor" id="NewEventFormSnippet":::

    Dadurch wird der Seite ein Formular hinzugefügt, damit der Benutzer Werte für das neue Ereignis eingeben kann.

1. Fügen Sie den folgenden Code am Ende der Datei hinzu.

    :::code language="razor" source="../demo/GraphTutorial/Pages/NewEvent.razor" id="NewEventCodeSnippet":::

    Überprüfen Sie die Funktionsweise dieses Codes.

    - In `OnInitializedAsync` wird die Zeitzone des authentifizierten Benutzers abgerufen.
    - In `CreateEvent` IT wird ein neues **Event** -Objekt unter Verwendung der Werte aus dem Formular initialisiert.
    - Das Ereignis wird mithilfe des Graph-SDK dem Kalender des Benutzers hinzugefügt.

1. Speichern Sie alle Änderungen, und starten Sie die App neu. Wählen Sie auf der Seite **Kalender** die Option **Neues Ereignis** aus. Füllen Sie das Formular aus, und wählen Sie **Erstellen** aus.

    ![Screenshot des neuen Ereignis Formulars](images/create-event.png)
