---
ms.openlocfilehash: 723f1d08b9d1085d47d0d5fac71da5371badc3d0
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584574"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="544f4-101">In diesem Abschnitt können Sie die Möglichkeit zum Erstellen von Ereignissen im Kalender des Benutzers hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="544f4-101">In this section you will add the ability to create events on the user's calendar.</span></span>

1. <span data-ttu-id="544f4-102">Erstellen Sie eine neue Datei im **./Pages** -Verzeichnis mit dem Namen "New **Event. Razor** ", und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="544f4-102">Create a new file in the **./Pages** directory named **NewEvent.razor** and add the following code.</span></span>

    :::code language="razor" source="../demo/GraphTutorial/Pages/NewEvent.razor" id="NewEventFormSnippet":::

    <span data-ttu-id="544f4-103">Dadurch wird der Seite ein Formular hinzugefügt, damit der Benutzer Werte für das neue Ereignis eingeben kann.</span><span class="sxs-lookup"><span data-stu-id="544f4-103">This adds a form to the page so the user can enter values for the new event.</span></span>

1. <span data-ttu-id="544f4-104">Fügen Sie den folgenden Code am Ende der Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="544f4-104">Add the following code to the end of the file.</span></span>

    :::code language="razor" source="../demo/GraphTutorial/Pages/NewEvent.razor" id="NewEventCodeSnippet":::

    <span data-ttu-id="544f4-105">Überprüfen Sie die Funktionsweise dieses Codes.</span><span class="sxs-lookup"><span data-stu-id="544f4-105">Consider what this code does.</span></span>

    - <span data-ttu-id="544f4-106">In `OnInitializedAsync` wird die Zeitzone des authentifizierten Benutzers abgerufen.</span><span class="sxs-lookup"><span data-stu-id="544f4-106">In `OnInitializedAsync` it gets the authenticated user's time zone.</span></span>
    - <span data-ttu-id="544f4-107">In `CreateEvent` IT wird ein neues **Event** -Objekt unter Verwendung der Werte aus dem Formular initialisiert.</span><span class="sxs-lookup"><span data-stu-id="544f4-107">In `CreateEvent` it initializes a new **Event** object using the values from the form.</span></span>
    - <span data-ttu-id="544f4-108">Das Ereignis wird mithilfe des Graph-SDK dem Kalender des Benutzers hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="544f4-108">It uses the Graph SDK to add the event to the user's calendar.</span></span>

1. <span data-ttu-id="544f4-109">Speichern Sie alle Änderungen, und starten Sie die App neu.</span><span class="sxs-lookup"><span data-stu-id="544f4-109">Save all of your changes and restart the app.</span></span> <span data-ttu-id="544f4-110">Wählen Sie auf der Seite **Kalender** die Option **Neues Ereignis** aus.</span><span class="sxs-lookup"><span data-stu-id="544f4-110">On the **Calendar** page, select **New event**.</span></span> <span data-ttu-id="544f4-111">Füllen Sie das Formular aus, und wählen Sie **Erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="544f4-111">Fill in the form and choose **Create**.</span></span>

    ![Screenshot des neuen Ereignis Formulars](images/create-event.png)
