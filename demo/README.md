---
ms.openlocfilehash: 9029486df6b08042681c0a1a67b156cbbd0381a0
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584608"
---
# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="ea6f4-101">Vorgehensweise Ausführen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="ea6f4-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ea6f4-102">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="ea6f4-102">Prerequisites</span></span>

<span data-ttu-id="ea6f4-103">Um das abgeschlossene Projekt in diesem Ordner auszuführen, benötigen Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="ea6f4-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="ea6f4-104">Das [.net Core SDK](https://dotnet.microsoft.com/download) , das auf Ihrem Entwicklungscomputer installiert ist.</span><span class="sxs-lookup"><span data-stu-id="ea6f4-104">The [.NET Core SDK](https://dotnet.microsoft.com/download) installed on your development machine.</span></span> <span data-ttu-id="ea6f4-105">(**Hinweis:** dieses Lernprogramm wurde mit .net Core SDK Version 3.1.402 geschrieben.</span><span class="sxs-lookup"><span data-stu-id="ea6f4-105">(**Note:** This tutorial was written with .NET Core SDK version 3.1.402.</span></span> <span data-ttu-id="ea6f4-106">Die Schritte in diesem Leitfaden funktionieren möglicherweise mit anderen Versionen, aber das wurde nicht getestet.)</span><span class="sxs-lookup"><span data-stu-id="ea6f4-106">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="ea6f4-107">Entweder ein persönliches Microsoft-Konto mit einem Postfach auf Outlook.com oder ein Microsoft-Arbeits-oder Schulkonto.</span><span class="sxs-lookup"><span data-stu-id="ea6f4-107">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="ea6f4-108">Wenn Sie kein Microsoft-Konto haben, gibt es mehrere Optionen, um ein kostenloses Konto zu erhalten:</span><span class="sxs-lookup"><span data-stu-id="ea6f4-108">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="ea6f4-109">Sie können [sich für ein neues persönliches Microsoft-Konto registrieren](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="ea6f4-109">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="ea6f4-110">Sie können sich [für das Office 365 Entwicklerprogramm registrieren](https://developer.microsoft.com/office/dev-program) , um ein kostenloses Office 365-Abonnement zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="ea6f4-110">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="ea6f4-111">Registrieren einer Webanwendung im Azure-Active Directory Admin Center</span><span class="sxs-lookup"><span data-stu-id="ea6f4-111">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="ea6f4-112">Öffnen Sie einen Browser, und navigieren Sie zum [Azure Active Directory Admin Center](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ea6f4-112">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="ea6f4-113">Melden Sie sich mit einem **persönlichen Konto** (auch: Microsoft-Konto) oder einem **Geschäfts- oder Schulkonto** an.</span><span class="sxs-lookup"><span data-stu-id="ea6f4-113">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="ea6f4-114">Wählen Sie in der linken Navigationsleiste **Azure Active Directory** aus, und wählen Sie dann **App-Registrierungen** unter **Verwalten** aus.</span><span class="sxs-lookup"><span data-stu-id="ea6f4-114">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="ea6f4-115">Screenshot der APP-Registrierungen</span><span class="sxs-lookup"><span data-stu-id="ea6f4-115">A screenshot of the App registrations</span></span> ](../tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="ea6f4-116">Wählen Sie **Neue Registrierung** aus.</span><span class="sxs-lookup"><span data-stu-id="ea6f4-116">Select **New registration**.</span></span> <span data-ttu-id="ea6f4-117">Legen Sie auf der Seite **Anwendung registrieren** die Werte wie folgt fest.</span><span class="sxs-lookup"><span data-stu-id="ea6f4-117">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="ea6f4-118">Legen Sie **Name** auf `Blazor Graph Tutorial` fest.</span><span class="sxs-lookup"><span data-stu-id="ea6f4-118">Set **Name** to `Blazor Graph Tutorial`.</span></span>
    - <span data-ttu-id="ea6f4-119">Legen Sie **Unterstützte Kontotypen** auf **Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten** fest.</span><span class="sxs-lookup"><span data-stu-id="ea6f4-119">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="ea6f4-120">Legen Sie unter **Umleitungs-URI** die erste Dropdownoption auf `Web` fest, und legen Sie den Wert auf `https://localhost:5001/authentication/login-callback` fest.</span><span class="sxs-lookup"><span data-stu-id="ea6f4-120">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `https://localhost:5001/authentication/login-callback`.</span></span>

    ![Screenshot der Seite "Anwendung registrieren"](../tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="ea6f4-122">Wählen Sie **Registrieren** aus.</span><span class="sxs-lookup"><span data-stu-id="ea6f4-122">Select **Register**.</span></span> <span data-ttu-id="ea6f4-123">Kopieren Sie auf der Seite " **Blazer Graph Tutorial** " den Wert der **Anwendungs-ID (Client)** , und speichern Sie Sie, um Sie im nächsten Schritt zu benötigen.</span><span class="sxs-lookup"><span data-stu-id="ea6f4-123">On the **Blazor Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Screenshot der Anwendungs-ID der neuen App-Registrierung](../tutorial/images/aad-application-id.png)

1. <span data-ttu-id="ea6f4-125">Wählen Sie unter **Verwalten** die Option **Authentifizierung** aus.</span><span class="sxs-lookup"><span data-stu-id="ea6f4-125">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="ea6f4-126">Suchen Sie den **impliziten Grant** -Abschnitt, und aktivieren Sie **Zugriffstoken** und **ID-Token**.</span><span class="sxs-lookup"><span data-stu-id="ea6f4-126">Locate the **Implicit grant** section and enable **Access tokens** and **ID tokens**.</span></span> <span data-ttu-id="ea6f4-127">Wählen Sie **Speichern** aus.</span><span class="sxs-lookup"><span data-stu-id="ea6f4-127">Select **Save**.</span></span>

## <a name="configure-the-sample"></a><span data-ttu-id="ea6f4-128">Konfigurieren des Beispiels</span><span class="sxs-lookup"><span data-stu-id="ea6f4-128">Configure the sample</span></span>

1. <span data-ttu-id="ea6f4-129">Benennen Sie die **/GraphTutorial/wwwroot/-appsettings.example.jsfür** die Datei in **appsettings.jsein**.</span><span class="sxs-lookup"><span data-stu-id="ea6f4-129">Rename the **/GraphTutorial/wwwroot/appsettings.example.json** file to **appsettings.json**.</span></span>

1. <span data-ttu-id="ea6f4-130">Bearbeiten Sie **appsettings.js** und ersetzen `YOUR_APP_ID_HERE` Sie Sie durch Ihre Anwendungs-ID.</span><span class="sxs-lookup"><span data-stu-id="ea6f4-130">Edit **appsettings.json** and replace `YOUR_APP_ID_HERE` with your application ID.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="ea6f4-131">Ausführen des Beispiels</span><span class="sxs-lookup"><span data-stu-id="ea6f4-131">Run the sample</span></span>

<span data-ttu-id="ea6f4-132">Führen Sie in der CLI den folgenden Befehl aus, um die Anwendung zu starten.</span><span class="sxs-lookup"><span data-stu-id="ea6f4-132">In your CLI, run the following command to start the application.</span></span>

```Shell
dotnet run
```
