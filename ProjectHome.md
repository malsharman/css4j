This project is an implementation of W3C's [CSS Object Model API](http://www.w3.org/TR/DOM-Level-2-Style/css.html) in the Java<sup>(TM)</sup> language, and adds CSS support to the [DOM4J](http://dom4j.sourceforge.net/) package.

Its main drawback is the lack of good [CSS3 support](#CSS3_Support.md).

## Overview ##
This implementation can be used in several ways: with stand-alone style sheets, combined with [DOM4J](http://dom4j.sourceforge.net/), or by manually building a [DOM](http://www.w3.org/DOM/) tree.

You can play with independent style sheets created with the `createStyleSheet("`_title_`", "`_media_`")` method of the `DOMCSSStyleSheetFactory` class, or the `DOM4JCSSStyleSheetFactory` if you use DOM4J. The resulting style sheets are empty, but you can load a style sheet with the `AbstractCSSStyleSheet.parseCSSStyleSheet(source)` method.

To obtain the 'computed' or 'used' values required for actual rendering, the library uses the `DeviceFactory` interface to supply device and media-specific information, also providing the objects required by media queries to work (not yet implemented).

### Short Example with DOM4J ###
This is the easiest way to use this package with DOM4J:
```
   Reader re = ...  [reader for XHTML document]
   InputSource source = new InputSource(re);
   SAXReader reader = new SAXReader(XHTMLDocumentFactory.getInstance());
   reader.setEntityResolver(new DefaultEntityResolver());
   XHTMLDocument document = (XHTMLDocument) reader.read(source);
```
And once you got the element you want style for (see, for example, the [DOM4J Guide](http://dom4j.sourceforge.net/dom4j-1.6.1/guide.html)), just get it with a procedure analogous to the [ViewCSS interface](http://www.w3.org/TR/DOM-Level-2-Style/java-binding.html#org.w3c.dom.css.ViewCSS):
```
   CSSStyleDeclaration style = ((CSSStylableElement)element).getComputedStyle(null);
   String propertyValue = style.getPropertyValue("display");
```
Be careful to have the `XHTMLDocumentFactory` loaded with the desired defaults, like the user agent style sheet; for example an XHTML 1.1 user agent sheet is loaded by default and some users do not expect that. The use with DOM4J is described in a bit more detail in the description for the `doc.dom4j` subpackage in the Javadocs.

### Usage with Generic DOM ###
If you choose to build your own DOM Document instead of using DOM4J, you can use the `DOMCSSStyleSheetFactory.createCSSDocument(document)` method to wrap a pre-existing DOM Document.

### Media Handling ###
By default, computed styles only take into account generic styles that are common to all media. If you want to target a more specific medium, you have to use the `CSSDocument.setTargetMedium("`_medium_`")` method.

This way to tie a document with a medium is not totally standard, as the W3C APIs would probably expect a `DeviceFactory`-related object implementing the [ViewCSS interface](http://www.w3.org/TR/DOM-Level-2-Style/java-binding.html#org.w3c.dom.css.ViewCSS) and referencing the document, but allows to isolate DOM logic inside DOM objects and keep the `DeviceFactory` for media-specific information only.

## CSS3 Support ##
CSS level 3 is only partially supported by the library; the following table summarizes the basic support for setting/retrieving the main CSS3 features:

| **CSS3 Spec Name** | **Support** | **Comments** |
|:-------------------|:------------|:-------------|
| Background / Border | Yes         |              |
| Color              | Yes         |              |
| Media Queries      | No          | There is already some basic infrastructure for media queries in some classes/interfaces, but it is not yet in use. |
| Selectors          | No          | Modern selectors are unsupported by the available SAC parsers. Anything but simple Level 2 selectors should be considered risky. |
| Transitions        | Yes         |              |
| Values             | Partial     | [W3C's SAC](http://www.w3.org/Style/CSS/SAC/) implementation does not support new level 3 units, and current SAC parsers provide somewhat limited support for new types; for example `calc()` values will be ignored by Batik 1.7. |

## Java<sup>(TM)</sup> Runtime Environment Requirements ##
All the classes in the binary package have been compiled with a Java SE version 7 compiler with 1.6 compiler compliance level.

## Dependencies ##
  * [SAC: Simple API for CSS](http://www.w3.org/Style/CSS/SAC/).
  * A SAC-compliant parser like [Batik SVG toolkit](http://xml.apache.org/batik/). Only files batik-css.jar and batik-util.jar are required.
  * The [JCLF](http://sourceforge.net/projects/jclf/) base library (JCLF's listed dependency is not used and can be ommitted).
  * The [SLF4J](http://www.slf4j.org/) logging library, basically used by the error-reporting components in the sample user agents.
  * If you use the DOM4J part you need:
    1. The [DOM4J](http://dom4j.sourceforge.net/) library.
    1. [Jaxen](http://jaxen.codehaus.org/).

## Project Status ##
This project is no longer being developed.

> <sub>WARNING: Please note that Sourceforge's 'css4j' project is a different project that decided to use the same name as this (pre-existing) one. This project was the first to use the name and publish source code, in February of 2006. The 'other' css4j was created in October of that year.</sub>