---
title: Complex Sum in XSLT
---

When generating reports from XML data using XSLT I have to perform complex mathematical operations. 
Or, rather, operations that generic XPath can't do. To achive that you can use variables in XSLT and fill node lists with them. So we have:

```
<xsl:variable name="totalCommission">
 <xsl:for-eachselect="orderz/accounts/payment/orderzs[@o_status != -9 and @o_prodtype != 'J']">
  <accum><xsl:value-of select="(@o_extprice  - @o_discount -itemmaster/itempricelist/@storecost*@qty) * ../../../../@salescommission div 100"/></accum>
 </xsl:for-each>
</xsl:variable>
```

as a result we create a variable that generates its element using some algorithm. After that, we can run a sum operation against it:

```
$<xsl:value-ofselect="format-number(sum(msxsl:node-set($totalCommission)/accum),'#,##0.00')" />
```

Very important in this operation is support of node set generator msxsl:node-set(), which is supported in MSXML that i use in this case.
To use it the root XML node will look like this:

```
<xsl:stylesheet xmlns:xsl="http://www.w3.org/1999/XSL/Transform"version="1.0" xmlns:msxsl="urn:schemas-microsoft-com:xslt">
```