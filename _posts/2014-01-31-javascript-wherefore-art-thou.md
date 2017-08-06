---
title: JavaScript, wherefore art thou
---


Standing in the shower this morning I was thinking about the past and the future. How the world changed around me, and how I changed. About happiness and fulfilling life. Then I thought about my browser – hey, it’s the shower – anything goes.

# Mosaic 2.0 – Hi Mozilla – 1995
My first exposure to the web happened via my dads work. They have purchased dial-up plan from one of first ISPs in Moscow – Glasnet. I am sure it cost something ridiculous.

<!--break-->

Fun fact – this was my first exposure to 419 email scam too, back in 1995. Later I learned this has been going on via fax for years prior to that.

So what does a young person do back then – remember, no knowledge of web. Purchase a book of course – a crude translation of some “book about internet”. Naturally it talked about all valuable telnet, ftp and gopher resources.  And of course some web. Once I installed Mosaic the internet did turn into something visual and useful – it was awesome.  I remember sitting in the factory office, looking at some astronomy paper on the web. And clicking links! Over time I did switch to Netscape 2.0.

Chronologically, in 1994 almost 100% of my time was spent in MS-DOS developing in TurboPascal or Turbo C++ . Next year, Windows 95 comes out. So I jumped from the world of 286 (and sometimes even 8086) computers straight to the web.

# Netscape JavaScript 1.0 book – 1996
In the summer of 1996 I started part-time work at ISA.ru. The first thing I was given was Sun’s Netra server – with all the glorious Solaris software (like CDE and Netscape Enterprise Server). I had zero knowledge of web development – but it was interesting that Sun included a couple of books on HTML and Netscape JavaScript.

This was an new experience reading these, of course – first reference book in English. What I still vividly remember what it actually covered – all the JavaScript could do back then is work with document.forms collection. Netscape 2.0 was the first version to start supporting JavaScript (first called LiveScript) back in 1995.

At this time, I also got a hold of my schools subscription of Wired magazines. Oh boy, was it cool back then. Wired basically invented the vivid, metallic and font-crazy layout. But of course it was the articles that you read the magazines for. It really opened my eyes to what was happening with the web.

# IE 3.0 and Netscape 4.0 – 1997
Prior to 3.0, IE was basically licensed Mosaic.  Remember, Bill Gates didn’t believe in the Internet – MSN was the way of online.

Also, 3.0 was bundled with Win 95 SP2 (the good win 95, so called OSR2) so it started the first browser war and decline of Netscape. The important milestone for JavaScript here is introduction of JScript – actual support for JavaScript in Microsoft’s own product.

Netscape releases 4.0 (Communicator) 6 months later. To me, this was a major change – this became my main browser for years to come (until Mozilla). Lots of time spent actually developing web sites for it.

# IE 4.0 – 1998
This same year, IE 4 is released. This was included in Windows 98 next year.

While I wasn’t aware of the internal changes at the time, in retrospect this is probably the main change in IE and web. It was called the Trident engine. In fact, it’s still used to date in IE on all platforms. It introduced proper DOM and DOM manipulation – which was really a horrible experience in Netscape.

In 1999, next version 5.0 included support for AJAX. So basically you could create modern web in 1999. Which in fact was done by OWA in 2000. The OWA team created the original xmlhttp ActiveX component, which was then included in IE 5.0 as Microsoft.XMLHTTP.

# Layers vs divs… neither! – 1999
At that point the future of dynamic HTML was not clear. And everybody wanting to create a dynamic page had to support both IE’s DOM and Netscapes Layers. Not fun, as they work completely differently (and Layers was buggy).

The only cross-platform solution was to regenerate the whole page using document.clear() and document.write(). While killing your own javascript of course. So this only works with … frames! Javascript from one frame can overwrite HTML of another frame. That was the standard of dynamic pages back then – for example web chats used this technique.

# Netscape 6 – 2000
Mozilla project is a rewrite of Netscape’s code. It has brand new Gecko engine, plus a ton of inherited code for all other parts of the browser (security, networking etc.). It reached the 0.6 – I was already trying it out as part of Mozilla Suite. And it was horribly broken.

At this point, I moved to US and started work as web developer – so I finally got exposed to all the crazy things in NN4. Gecko was a huge hope and breath of fresh air. But it simply wasn’t ready – AOL releasing production NN6 based on 0.6 of Gecko was an act of desperation.

# IE 6.0 stagnation – 2001-2006
When IE 6.0 released with Windows XP,  Netscape was completely dead. IE effective share is 90%. Mozilla 1.0 is one year away. 

The web developers concentrated on supporting NN4 and developing for IE6. This is why we still have to deal with it – a huge amount of investment was made between 2001 and Mozilla finally taking some meaningful share of the market with Firefox in 2005.

The JavaScript and AJAX support is OK. But layout are horribly broken, as any CSS developer knows.

# Mozilla 1.0 – let’s party? – 2002
When Mozilla finally released in 2002 I was so so happy. Even went to one of release parties in Cambridge, Mass. We thought that all we had to do now was wait for the adoption. Didn’t happen – why would anybody use some other browser? Remember, IE was completely dominant both for consumers and developers.

# Phoenix, I mean Firebird … ok ok, Firefox! – 2003-2005
Changing name 3 times is not awesome for a project that has market share as its main goal, right? I mean come on, you are developers – pick some non-conflicting name.

I actually didn’t switch to Firefox until much later, when it became obvious Mozilla Suite wouldn’t get updated.

In 2004, once Firefox got off the ground, the www.spreadfirefox.com was launched – and it was awesome. We actually got a full page ad for Firefox in New York Times! https://blog.mozilla.org/community/2013/06/10/milestone-ad-for-firefox-in-new-york-times/

Finally in 2005 IEs market share began to erode. A combination of factors really affected that:

* XP was unsecure with IE, and Vista failed in 2006. Security on the internet becomes a huge concern for users. Even Microsoft switches to firefighting mode with security push in 2002 and releases major architectural security changes in XP SP2 in 2004.
* IE had no major releases until… wow, 2006! And realistically, for developers, until 2009 with 8.0.
* Gecko finally caught up with IE if you can call it that. Sites actually rendered the same. Developers started to be more standards-aware. A good example is document.all support in Gecko – it’s an interesting way to work around the issue. You can’t check document.all for existence. But if you do document.all[“foo”] then you do get a result.
* Web 2.0 starts – just in 2005 we saw Facebook and YouTube. Browsing has to become better – and Firefox delivers: speed, very useful addons, security.

# KHTML – a what what? – 2003-2007
Apple (which just made iPods and Macs at that point) decides to make their own browser. And they don’t pick an easily customizable Mozilla for that, blasphemy! They picked some broken part of KDE for that.

Little did we know, they were working on web tablet since 2002. This was part of the project. Of course, eventually they decided to ship smaller factor first (iPhone).

4 years into the project, iOS finally ships. And Webkit instantly becomes best and fastest mobile browser. Poor Opera.

# So it’s OK to build your own browser, Google Chrome steps in 2008
The main fear when Google started their own browser was the fate of Firefox. They were giving tons of money to Mozilla to have Google as default search engine at the time. 6 years later, they are still paying.

The innovation that Chrome brought was self-updating. After all, if the site can self-update why can’t the browser. Firefox, after fighting with old schoolers finally adopted the model too. And so did Microsoft, at least partially – IE now installs automatically as part of Windows Update. Previously it had it’s own terms you had to accept and was an optional update.

# OMG, let’s catch up! IE9 Chakra – 2011
The browser world is moving like crazy – since 2007, there are Firefoxes, Chromes, Safaris and Operas eating into IE market share. Nice touch mobile browsing is growing. By the end of 2010, Internet Explorer is under 50%.

After playing around with .NET and Silverlight the writing is on the wall for Microsoft – they have to embrace the web. The old layout and javascript engines have to go. Browser has to update separate from OS and much more frequently. It has to work on mobile. Microsoft got to work in 2009.

As is traditional for Microsoft, when they invest into a rewrite they really over-invest – compared to gradual progression of other browsers. Everything is different:

Hardware rendering (requiring Vistas DirectX – so no XP compatibility)
New JITed JavaScript engine – very very fast
New layout engine – finally Acid support
New standards support – SVG, HTML5 video, canvas, WOFF
It helped lay the foundation which is still used for IE11 and up, with more standards and compatibility being added. And it’s a great mobile browser too. Windows 8 also includes an application runtime based on IE9 which has no browser restrictions – alas, that runtime is not present on Phone edition, so you have to keep using PhoneGap / Cordova.

# Finally, 10 years later, new standards emerge
It’s not W3Cs fault – after all they simply represent the industry, they can’t force anything. But the standards that were being developed – XHTML and XHTML2 were made in vacuum, trying to make existing markup nicer. The world wasn’t interested in making web apps – XBAP, Silverlight and Flash were the future.

In 2004 a small group of developers actually interested in adding features to HTML formed, and started Web Hypertext Application Technology Working Group (WHAT-WG). Initially we mainly wanted to improve forms, so it was an alternative to W3C XForms standard. But eventually, this morphed into full-fledged next version of HTML. A practical HTML spec actually useful for making the apps.

It was approved by W3C in 2007 and could not have happened if IE still had 90% market share. By 2007 it actually had vendor support (Mozilla, Apple and Opera).

So finally in 2011, 10 years after IE took over, developers actually got new APIs to use.

# Now
This is hard topic to talk about. But 2 years into web app revolution, we are only starting to scratch the surface of “normal” application development. Chrome Development Tools are becoming a part of IDE. Easy mobile debugging only starting to show up. Profiling tools are finally maturing in Chrome and IE11. Automated testing, especially on mobile, is still painful but getting better.

The great story is the frameworks and libraries. Between polyfills, OR/M, MVC, graphical libraries, packaging, unit testing, and even server-side javascript – it’s hard to pick what’s more awesome.  My favorite success story is Sencha which went from single person GPL project to a 150 person company doing nothing but javascript framwork.

# A look into next 10 years
Mobile will be majority of browsing – it already is depending on where and how you count. For example, many very popular apps use browser component. Additionally, mobile share is extremely high outside of US. On the business front this may take all of 10 years, but already the business software that is currently being developed is all web.  Every time the market share conversation starts, people talk about business/desktop segment – and sure, there will be emulators to run the current non-web apps. Same as we have emulators, and sometimes even wrappers, to run terminal apps.

Web is finally a standard now – a standard in a sense that every platform has to do it equally well. This sets a very high bar for entry – so making a new mobile OS is hard. Previously, you could embed WebKit easily – the way Blackberry has done it. However, with Blink branching, it is highly likely that WebKit will become Apple-only. They can’t close-source it, but they can make it un-portable. Same with Blink. The poor Mobile Firefox is limited to Android, but Chrome already ships with OS.

This leaves us with triumvirate of IOS WebKit, Android Blink and Windows IE. With Android becoming the windows of mobile (by market share), Blink will be the web development platform. It is the new IE. There are of course differences – the update cycle on phones is pretty fast, Google likes to push web standards. We may even see Blink self-updating on android. It looks like the turbulent period of having many browsers is coming to an end. The web apps in the next 10 years will concentrate on supporting Blink and polyfilling into WebKit and IE.

What innovation can then happen in the browser space? Thanks to Chrome the UI of the browsers already went away, and so did the support for additional functionality like RSS. The innovation will be all under the hood, invisible to consumers. Browser simply became the platform for development, similar to Flash or Silverlight – and these hardly had any UI.

What can we expect? Short term is obvious – all browsers getting support for all current standards like WebRTC and some upcoming like web components. Long term, we will get new standards for everything previously invented. After all it’s a platform, and you need to do everything – like memory management, raw networking, hardware access, and multi-threading. Maybe even a different/better language. Maybe even bytecode support  for JavaScript.

Really, pretty boring stuff. But boring is good – I would rather concentrate on making the platform much easier to develop for. And here we finally come to critical point – hey, we have a single platform? For the first time in computing, we have a single connected platform to develop apps for? Apps that can run on any size screen, and any type of device?  The unfulfilled promise of the past? This platform self-assembled and evolved from the horrors of the browser wars.

For the next 10 years, we welcome our Blink overlords.