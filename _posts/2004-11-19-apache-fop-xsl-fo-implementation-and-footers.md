---
title: Apache FOP XSL-FO implementation and footers
---

"flows" in XSL-FO can have static areas surrounding them, which are
naturaly used to make headers and footers on pages. Let's consider
sales invoice like this:

sample invoice

Here, of course, you can position header and footer absolutely using
```
<fo:block-container left="xx" top="xx" position="absolute">.
```
However, when your invoice grows and you run out of space on the page,
and you want to see something like this:

invoice how it should page correctly

<!--break-->

In XSL-FO standard this is achived using special static sections of the page:

```
<fo:page-sequence-master master-name="all-pages">
  <fo:repeatable-page-master-alternatives>
    <fo:conditional-page-master-reference 
            page-position="any" master-reference="any-page-id"/>
    <fo:conditional-page-master-reference 
            page-position="last" master-reference="last-page-id"/>
  </fo:repeatable-page-master-alternatives>
</fo:page-sequence-master>

```

Here you use master-reference parameter values your page master definition , for example:

```
<fo:simple-page-master master-name="last-page-id">
```

Inside that master you can define different footer. A good example is given here http://www.xmlpdf.com/ibex-examples.html


Problem is, tracking which page is "last" is very tricky in
free-flow documents. Take HTML for example, the common question about
how to make something 100% high is hard to answer. The content can flow
differently, and, more importantly, the element that is trying to make
use of that position can affect its own position. For example, now in
XSL-FO, a footer on the last page can be so high that the engine will
have to create another page after the page that was considered "last"
just to fit it. In which case, page before last will suddenly fit all
content on the last page too, because it doesn't have footer anymore.
So engine has to make a decision on how to fix that in one or another
position! What I am getting to is that Apache FOP doesn't do that yet,
and in general many complex things having to do with flow. It's working
just fine with everything else it seems. This is why current version is
in "maintanence" now and developers are working on redesigning for next
version (which doesn't have name yet, but either 0.30 or 1.0 


So what do you do in the mid time? Well, you have 3 workarounds
(none of which is perfect). First one that is normally suggested is to
use fo:footnote element to emulate the footer on the last page. Don't
forget that you have to have footnote reference text, otherwise it
won't generate anything:

```
<fo:block id="end">
 <fo:footnote><fo:inline color="white">.</fo:inline>
 <fo:footnote-body>...
```
The problem with footnote is that you can't use absolutely
positioned blocks in it, and it seems to only like tables for fine
formatting. So the example that i gave above on the picture might be
hard to implement.


The second workaround is to use some code inside your program
(before FOP call) to tell which items will be on last page, and put
them into a different page master. That may be hard for some
applications, again, like in example I gave, each line in the invoice
can have wrapping text and as a result starts taking more space than
originally estimated.


The last solution is not perfect either - don't make special
footer.This is what I ended up doing (and it was fine in my case, since
every invoice page has same footer in my clients case). So in the end
it looks like this: ...


You may think of switching to commercial XSL-FO generator, like
RenderX or Antenna House, which seem to support flows better, and
overall are high quality commercial products.

http://www.renderx.net/Content/tools/xep.html

http://www.antennahouse.com/product.htm

For my own software I am thinking of supporting many different
renderers, based on clients ability to buy commercial renderer or to
live with Apache FOP limitations (which will be fixed some day).