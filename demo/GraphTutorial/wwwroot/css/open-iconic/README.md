---
ms.openlocfilehash: 3965884c8e0d7116f5d8a42e6aefb0dc5be94f1e
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584571"
---
<a name="open-iconic-v111"></a>[<span data-ttu-id="da166-101">Öffnen Sie die Ikone v 1.1.1</span><span class="sxs-lookup"><span data-stu-id="da166-101">Open Iconic v1.1.1</span></span>](http://useiconic.com/open)
===========

### <a name="open-iconic-is-the-open-source-sibling-of-iconic-it-is-a-hyper-legible-collection-of-223-icons-with-a-tiny-footprintmdashready-to-use-with-bootstrap-and-foundation-view-the-collection"></a><span data-ttu-id="da166-102">Open Iconic ist das Open-Source-gleichgeordnete Element von [Iconic](http://useiconic.com).</span><span class="sxs-lookup"><span data-stu-id="da166-102">Open Iconic is the open source sibling of [Iconic](http://useiconic.com).</span></span> <span data-ttu-id="da166-103">Es handelt sich um eine Hyper-lesbare Sammlung von 223-Symbolen mit einem kleinen Footprint &mdash; , der mit Bootstrap und Foundation verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="da166-103">It is a hyper-legible collection of 223 icons with a tiny footprint&mdash;ready to use with Bootstrap and Foundation.</span></span> [<span data-ttu-id="da166-104">Anzeigen der Sammlung</span><span class="sxs-lookup"><span data-stu-id="da166-104">View the collection</span></span>](http://useiconic.com/open#icons)



## <a name="whats-in-open-iconic"></a><span data-ttu-id="da166-105">Was ist in "Open Iconic"?</span><span class="sxs-lookup"><span data-stu-id="da166-105">What's in Open Iconic?</span></span>

* <span data-ttu-id="da166-106">223 Symbole wurden für eine Lesbarkeit von bis zu 8 Pixel entwickelt</span><span class="sxs-lookup"><span data-stu-id="da166-106">223 icons designed to be legible down to 8 pixels</span></span>
* <span data-ttu-id="da166-107">Super Leichte SVG-Dateien – 61,8 für die gesamte Gruppe</span><span class="sxs-lookup"><span data-stu-id="da166-107">Super-light SVG files - 61.8 for the entire set</span></span> 
* <span data-ttu-id="da166-108">SVG-Sprite &mdash; der moderne Ersatz für Symbol Schriftarten</span><span class="sxs-lookup"><span data-stu-id="da166-108">SVG sprite&mdash;the modern replacement for icon fonts</span></span>
* <span data-ttu-id="da166-109">Webfont (EOT, OpenType, SVG, ttf, WOFF), PNG-und WebP-Formate</span><span class="sxs-lookup"><span data-stu-id="da166-109">Webfont (EOT, OTF, SVG, TTF, WOFF), PNG and WebP formats</span></span>
* <span data-ttu-id="da166-110">Webfont-Stylesheets (einschließlich Versionen für Bootstrap und Foundation) in CSS-, Less-, scss-und Stylus-Formaten</span><span class="sxs-lookup"><span data-stu-id="da166-110">Webfont stylesheets (including versions for Bootstrap and Foundation) in CSS, LESS, SCSS and Stylus formats</span></span>
* <span data-ttu-id="da166-111">PNG-und WebP-Rasterbilder in 8px, 16px, Bund, Bund, 48px und 64px.</span><span class="sxs-lookup"><span data-stu-id="da166-111">PNG and WebP raster images in 8px, 16px, 24px, 32px, 48px and 64px.</span></span>


## <a name="getting-started"></a><span data-ttu-id="da166-112">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="da166-112">Getting Started</span></span>

#### <a name="for-code-samples-and-everything-else-you-need-to-get-started-with-open-iconic-check-out-our-icons-and-reference-sections"></a><span data-ttu-id="da166-113">Für Codebeispiele und alles andere, was Sie benötigen, um mit Open Iconic zu beginnen, lesen Sie unsere [Symbole](http://useiconic.com/open#icons) und [Referenz](http://useiconic.com/open#reference) Abschnitte.</span><span class="sxs-lookup"><span data-stu-id="da166-113">For code samples and everything else you need to get started with Open Iconic, check out our [Icons](http://useiconic.com/open#icons) and [Reference](http://useiconic.com/open#reference) sections.</span></span>

### <a name="general-usage"></a><span data-ttu-id="da166-114">Allgemeine Verwendung</span><span class="sxs-lookup"><span data-stu-id="da166-114">General Usage</span></span>

#### <a name="using-open-iconics-svgs"></a><span data-ttu-id="da166-115">Verwenden der SVGs von Open Iconic</span><span class="sxs-lookup"><span data-stu-id="da166-115">Using Open Iconic's SVGs</span></span>

<span data-ttu-id="da166-116">Wir möchten SVGs und denken, dass Sie die Möglichkeit zum Anzeigen von Symbolen im Internet darstellen.</span><span class="sxs-lookup"><span data-stu-id="da166-116">We like SVGs and we think they're the way to display icons on the web.</span></span> <span data-ttu-id="da166-117">Da Open Iconic nur grundlegende SVGs sind, empfehlen wir Ihnen, diese wie jedes andere Bild anzuzeigen (vergessen Sie das `alt` Attribut nicht).</span><span class="sxs-lookup"><span data-stu-id="da166-117">Since Open Iconic are just basic SVGs, we suggest you display them like you would any other image (don't forget the `alt` attribute).</span></span>

```
<img src="/open-iconic/svg/icon-name.svg" alt="icon name">
```

#### <a name="using-open-iconics-svg-sprite"></a><span data-ttu-id="da166-118">Verwenden des SVG-Sprites von Open Iconic</span><span class="sxs-lookup"><span data-stu-id="da166-118">Using Open Iconic's SVG Sprite</span></span>

<span data-ttu-id="da166-119">Open Iconic kommt auch in einem SVG-Sprite vor, mit dem Sie alle Symbole in der Gruppe mit einer einzigen Anforderung anzeigen können.</span><span class="sxs-lookup"><span data-stu-id="da166-119">Open Iconic also comes in a SVG sprite which allows you to display all the icons in the set with a single request.</span></span> <span data-ttu-id="da166-120">Es ist wie eine Symbolschriftart, ohne dass es sich um einen Hack handelt.</span><span class="sxs-lookup"><span data-stu-id="da166-120">It's like an icon font, without being a hack.</span></span>

<span data-ttu-id="da166-121">Das Hinzufügen eines Symbols von einem SVG-Sprite ist ein wenig anders als das, was Sie gewohnt sind, aber es ist immer noch ein Stück Kuchen.</span><span class="sxs-lookup"><span data-stu-id="da166-121">Adding an icon from an SVG sprite is a little different than what you're used to, but it's still a piece of cake.</span></span> <span data-ttu-id="da166-122">*Tipp: damit Ihre Symbole leicht in der Lage sind, empfehlen wir, eine allgemeine Klasse* `<svg>` hinzuzufügen. *-Tag und einen eindeutigen Klassennamen für jedes andere Symbol in der* `<use>` *-Tag.*</span><span class="sxs-lookup"><span data-stu-id="da166-122">*Tip: To make your icons easily style able, we suggest adding a general class to the* `<svg>` *tag and a unique class name for each different icon in the* `<use>` *tag.*</span></span>  

```
<svg class="icon">
  <use xlink:href="open-iconic.svg#account-login" class="icon-account-login"></use>
</svg>
```

<span data-ttu-id="da166-123">Größen Anpassungs Symbole benötigen nur grundlegendes CSS.</span><span class="sxs-lookup"><span data-stu-id="da166-123">Sizing icons only needs basic CSS.</span></span> <span data-ttu-id="da166-124">Alle Symbole sind in einem quadratischen Format, legen Sie also einfach das `<svg>` Tag mit gleicher Breite und Höhe fest.</span><span class="sxs-lookup"><span data-stu-id="da166-124">All the icons are in a square format, so just set the `<svg>` tag with equal width and height dimensions.</span></span>

```
.icon {
  width: 16px;
  height: 16px;
}
```

<span data-ttu-id="da166-125">Das Färben von Symbolen ist noch einfacher.</span><span class="sxs-lookup"><span data-stu-id="da166-125">Coloring icons is even easier.</span></span> <span data-ttu-id="da166-126">Sie müssen lediglich die `fill` Regel für das `<use>` -Tag festlegen.</span><span class="sxs-lookup"><span data-stu-id="da166-126">All you need to do is set the `fill` rule on the `<use>` tag.</span></span>

```
.icon-account-login {
  fill: #f00;
}
```

<span data-ttu-id="da166-127">Wenn Sie mehr über SVG-Sprites erfahren möchten, lesen Sie [den Leitfaden für Chris Coyier](http://css-tricks.com/svg-sprites-use-better-icon-fonts/).</span><span class="sxs-lookup"><span data-stu-id="da166-127">To learn more about SVG Sprites, read [Chris Coyier's guide](http://css-tricks.com/svg-sprites-use-better-icon-fonts/).</span></span>

#### <a name="using-open-iconics-icon-font"></a><span data-ttu-id="da166-128">Verwenden der Symbolschriftart von Open Iconic...</span><span class="sxs-lookup"><span data-stu-id="da166-128">Using Open Iconic's Icon Font...</span></span>


##### <a name="with-bootstrap"></a><span data-ttu-id="da166-129">... mit Bootstrap</span><span class="sxs-lookup"><span data-stu-id="da166-129">…with Bootstrap</span></span>

<span data-ttu-id="da166-130">Unsere Bootstrap-Stylesheets finden Sie unter `font/css/open-iconic-bootstrap.{css, less, scss, styl}`</span><span class="sxs-lookup"><span data-stu-id="da166-130">You can find our Bootstrap stylesheets in `font/css/open-iconic-bootstrap.{css, less, scss, styl}`</span></span>


```
<link href="/open-iconic/font/css/open-iconic-bootstrap.css" rel="stylesheet">
```


```
<span class="oi oi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="with-foundation"></a><span data-ttu-id="da166-131">... mit Foundation</span><span class="sxs-lookup"><span data-stu-id="da166-131">…with Foundation</span></span>

<span data-ttu-id="da166-132">Unsere Foundation-Stylesheets finden Sie unter `font/css/open-iconic-foundation.{css, less, scss, styl}`</span><span class="sxs-lookup"><span data-stu-id="da166-132">You can find our Foundation stylesheets in `font/css/open-iconic-foundation.{css, less, scss, styl}`</span></span>

```
<link href="/open-iconic/font/css/open-iconic-foundation.css" rel="stylesheet">
```


```
<span class="fi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="on-its-own"></a><span data-ttu-id="da166-133">... eigenständig</span><span class="sxs-lookup"><span data-stu-id="da166-133">…on its own</span></span>

<span data-ttu-id="da166-134">Unsere Standard-Stylesheets finden Sie unter `font/css/open-iconic.{css, less, scss, styl}`</span><span class="sxs-lookup"><span data-stu-id="da166-134">You can find our default stylesheets in `font/css/open-iconic.{css, less, scss, styl}`</span></span>

```
<link href="/open-iconic/font/css/open-iconic.css" rel="stylesheet">
```

```
<span class="oi" data-glyph="icon-name" title="icon name" aria-hidden="true"></span>
```


## <a name="license"></a><span data-ttu-id="da166-135">Lizenz</span><span class="sxs-lookup"><span data-stu-id="da166-135">License</span></span>

### <a name="icons"></a><span data-ttu-id="da166-136">Symbole</span><span class="sxs-lookup"><span data-stu-id="da166-136">Icons</span></span>

<span data-ttu-id="da166-137">Der gesamte Code (einschließlich SVG-Markup) befindet sich unter der [mit-Lizenz](http://opensource.org/licenses/MIT).</span><span class="sxs-lookup"><span data-stu-id="da166-137">All code (including SVG markup) is under the [MIT License](http://opensource.org/licenses/MIT).</span></span>

### <a name="fonts"></a><span data-ttu-id="da166-138">Schriftarten</span><span class="sxs-lookup"><span data-stu-id="da166-138">Fonts</span></span>

<span data-ttu-id="da166-139">Alle Schriftarten sind unter dem [SIL lizensiert](http://scripts.sil.org/cms/scripts/page.php?item_id=OFL_web).</span><span class="sxs-lookup"><span data-stu-id="da166-139">All fonts are under the [SIL Licensed](http://scripts.sil.org/cms/scripts/page.php?item_id=OFL_web).</span></span>
