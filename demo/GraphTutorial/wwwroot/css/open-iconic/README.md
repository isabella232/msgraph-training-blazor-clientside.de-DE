---
ms.openlocfilehash: 3965884c8e0d7116f5d8a42e6aefb0dc5be94f1e
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584571"
---
<a name="open-iconic-v111"></a>[Öffnen Sie die Ikone v 1.1.1](http://useiconic.com/open)
===========

### <a name="open-iconic-is-the-open-source-sibling-of-iconic-it-is-a-hyper-legible-collection-of-223-icons-with-a-tiny-footprintmdashready-to-use-with-bootstrap-and-foundation-view-the-collection"></a>Open Iconic ist das Open-Source-gleichgeordnete Element von [Iconic](http://useiconic.com). Es handelt sich um eine Hyper-lesbare Sammlung von 223-Symbolen mit einem kleinen Footprint &mdash; , der mit Bootstrap und Foundation verwendet werden kann. [Anzeigen der Sammlung](http://useiconic.com/open#icons)



## <a name="whats-in-open-iconic"></a>Was ist in "Open Iconic"?

* 223 Symbole wurden für eine Lesbarkeit von bis zu 8 Pixel entwickelt
* Super Leichte SVG-Dateien – 61,8 für die gesamte Gruppe 
* SVG-Sprite &mdash; der moderne Ersatz für Symbol Schriftarten
* Webfont (EOT, OpenType, SVG, ttf, WOFF), PNG-und WebP-Formate
* Webfont-Stylesheets (einschließlich Versionen für Bootstrap und Foundation) in CSS-, Less-, scss-und Stylus-Formaten
* PNG-und WebP-Rasterbilder in 8px, 16px, Bund, Bund, 48px und 64px.


## <a name="getting-started"></a>Erste Schritte

#### <a name="for-code-samples-and-everything-else-you-need-to-get-started-with-open-iconic-check-out-our-icons-and-reference-sections"></a>Für Codebeispiele und alles andere, was Sie benötigen, um mit Open Iconic zu beginnen, lesen Sie unsere [Symbole](http://useiconic.com/open#icons) und [Referenz](http://useiconic.com/open#reference) Abschnitte.

### <a name="general-usage"></a>Allgemeine Verwendung

#### <a name="using-open-iconics-svgs"></a>Verwenden der SVGs von Open Iconic

Wir möchten SVGs und denken, dass Sie die Möglichkeit zum Anzeigen von Symbolen im Internet darstellen. Da Open Iconic nur grundlegende SVGs sind, empfehlen wir Ihnen, diese wie jedes andere Bild anzuzeigen (vergessen Sie das `alt` Attribut nicht).

```
<img src="/open-iconic/svg/icon-name.svg" alt="icon name">
```

#### <a name="using-open-iconics-svg-sprite"></a>Verwenden des SVG-Sprites von Open Iconic

Open Iconic kommt auch in einem SVG-Sprite vor, mit dem Sie alle Symbole in der Gruppe mit einer einzigen Anforderung anzeigen können. Es ist wie eine Symbolschriftart, ohne dass es sich um einen Hack handelt.

Das Hinzufügen eines Symbols von einem SVG-Sprite ist ein wenig anders als das, was Sie gewohnt sind, aber es ist immer noch ein Stück Kuchen. *Tipp: damit Ihre Symbole leicht in der Lage sind, empfehlen wir, eine allgemeine Klasse* `<svg>` hinzuzufügen. *-Tag und einen eindeutigen Klassennamen für jedes andere Symbol in der* `<use>` *-Tag.*  

```
<svg class="icon">
  <use xlink:href="open-iconic.svg#account-login" class="icon-account-login"></use>
</svg>
```

Größen Anpassungs Symbole benötigen nur grundlegendes CSS. Alle Symbole sind in einem quadratischen Format, legen Sie also einfach das `<svg>` Tag mit gleicher Breite und Höhe fest.

```
.icon {
  width: 16px;
  height: 16px;
}
```

Das Färben von Symbolen ist noch einfacher. Sie müssen lediglich die `fill` Regel für das `<use>` -Tag festlegen.

```
.icon-account-login {
  fill: #f00;
}
```

Wenn Sie mehr über SVG-Sprites erfahren möchten, lesen Sie [den Leitfaden für Chris Coyier](http://css-tricks.com/svg-sprites-use-better-icon-fonts/).

#### <a name="using-open-iconics-icon-font"></a>Verwenden der Symbolschriftart von Open Iconic...


##### <a name="with-bootstrap"></a>... mit Bootstrap

Unsere Bootstrap-Stylesheets finden Sie unter `font/css/open-iconic-bootstrap.{css, less, scss, styl}`


```
<link href="/open-iconic/font/css/open-iconic-bootstrap.css" rel="stylesheet">
```


```
<span class="oi oi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="with-foundation"></a>... mit Foundation

Unsere Foundation-Stylesheets finden Sie unter `font/css/open-iconic-foundation.{css, less, scss, styl}`

```
<link href="/open-iconic/font/css/open-iconic-foundation.css" rel="stylesheet">
```


```
<span class="fi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="on-its-own"></a>... eigenständig

Unsere Standard-Stylesheets finden Sie unter `font/css/open-iconic.{css, less, scss, styl}`

```
<link href="/open-iconic/font/css/open-iconic.css" rel="stylesheet">
```

```
<span class="oi" data-glyph="icon-name" title="icon name" aria-hidden="true"></span>
```


## <a name="license"></a>Lizenz

### <a name="icons"></a>Symbole

Der gesamte Code (einschließlich SVG-Markup) befindet sich unter der [mit-Lizenz](http://opensource.org/licenses/MIT).

### <a name="fonts"></a>Schriftarten

Alle Schriftarten sind unter dem [SIL lizensiert](http://scripts.sil.org/cms/scripts/page.php?item_id=OFL_web).
