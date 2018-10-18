# XML World-Countries XSLT Exercises

In these exercises, you will be working with a small XML data set about world countries. This data is adapted from the Mondial 3.0 database as hosted by the University of Washington, and was originally compiled by the Georg-August University of Goettingen Institute for Informatics. Each country has a name, population, and area (in sq. km). Some countries also list languages (with percentages of the population that speaks each language) and/or cities (with names and populations). The XML data is [here](https://prod-c2g.s3.amazonaws.com/db/Winter2013/files/countries.xml).

## Q1

Return all countries with population between 9 and 10 million. Retain the structure of country elements from the original data.

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<xsl:stylesheet version="2.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
<xsl:output method="xml" indent="yes" omit-xml-declaration="yes" />

<xsl:template match="country[@population &gt; 9000000 and @population &lt; 10000000]">
	<xsl:copy-of select="." />
</xsl:template>

<xsl:template match="text()" />

</xsl:stylesheet>
```

## Q2

Create a table using HTML constructs that lists all countries that have more than 3 languages. Each row should contain the country name in bold, population, area, and number of languages. Sort the rows in descending order of number of languages. No header is needed for the table, but use \<table border="1"\> to make it format nicely, should you choose to check your result in a browser. (**Hint**: You may find the data-type and order attributes of \<xsl:sort\> to be useful.)

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<xsl:stylesheet version="2.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
<xsl:output method="html" indent="yes" />

<xsl:template match="*">
	<html>
		<table border="1">
			<xsl:for-each select="country[count(language) &gt; 3]">
			<xsl:sort select="count(language)" order="descending" />
				<tr>
					<td><b><xsl:value-of select="@name" /></b></td>
					<td><xsl:value-of select="@population" /></td>
					<td><xsl:value-of select="@area" /></td>
					<td><xsl:value-of select="count(language)" /></td>
				</tr>
			</xsl:for-each>
	</table>
   </html>
</xsl:template>

</xsl:stylesheet>
```

## Q3

Create an alternate version of the countries database: for each country, include its name and population as sublements, and the number of languages and number of cities as attributes (called "languages" and "cities" respectively).

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<xsl:stylesheet version="2.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
<xsl:output method="xml" indent="yes" omit-xml-declaration="yes" />

<xsl:template match="*">
<countries>
	<xsl:for-each select="country">
		<country languages="{count(language)}" cities="{count(city)}">
			<name><xsl:value-of select="@name" /></name>
			<population><xsl:value-of select="@population" /></population>
		</country>
	</xsl:for-each>
</countries>
</xsl:template>

</xsl:stylesheet>
```
