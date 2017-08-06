---
title: Multilingual Apache FOP
---

So today we got one of our arabic clients request – customize the order template to include the arabic text in the captions and text areas.

First thing i did was to try and just give FOP russian text – that just gets output as # signs. To get this working you need to follow this good old tutorial – it mainly talks about doing it manually (what i was doing first). The more extended, and newer (for versions 0.93 and 94) article is naturally located on official site. You will especially need it when you want to do it from code, not command line.

The font that you can use in Windows which covers almost all languages (per unicode) is located at c:\windows\fonts\arialuni.ttf . It’s a huge (22Mb) font that generates a pretty big font metrics (about 700k for me). This file, however, is not standard in windows – it seems to be part of the Microsoft Office installation. None of our windows servers have it by default, for example. Or you can just use plain arial, which covers all european scripts.

Finally tried doing arabic script using this font technique. This doesn’t work, and it doesn’t look like it will work any time soon. The problem of course is BIDI (bi-directional text support) – FOP can use arabic characters, but can not reverse the direction (and one can not simply reverse character order before giving it to FOP – arabic non-capital letters have to be connected to each other in a very special way). There’s no bidi support in FOP (although XSL-FO does have it in the spec). The mailing archive hints to some people working on it successfully, however the major obstacle seems to be lack of proper BIDI support in Java 1.3 itself – only 1.4 added needed methods, but FOP wants to stay 1.3-compatible.

Update: well, I just got a notification of 0.95 beta, and it does drop the 1.3 requirement. Good news! Hopefully the custom work that is done by other developers will be included eventually.

We had to revert to a workaround – the areas where arabic is needed are going to be bitmaps that our customers will be able to upload the images for.