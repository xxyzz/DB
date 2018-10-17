# XML Course-Catalog XSLT Exercises

In these exercises, you will be working with a small XML data set drawn from the Stanford course catalog. There are multiple departments, each with a department chair, some courses, and professors and/or lecturers who teach courses. The XML data is [here](https://prod-c2g.s3.amazonaws.com/db/Winter2013/files/courses-noID.xml).

## Q1

Return a list of department titles.

Your solution should fill in the following stylesheet:

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<xsl:stylesheet version="2.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
    <xsl:template match=...>
        ... template body ...
    </xsl:template>
    ... more templates as needed ...
</xsl:stylesheet>
```

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<xsl:stylesheet version="2.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
<xsl:output method="xml" indent="yes" omit-xml-declaration="yes" />

<xsl:template match="Department">
    <Title> <xsl:value-of select="Title" /> </Title>
</xsl:template>

</xsl:stylesheet>
```

## Q2

Return a list of department elements with no attributes and two subelements each: the department title and the entire Chair subelement structure.

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<xsl:stylesheet version="2.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
<xsl:output method="xml" indent="yes" omit-xml-declaration="yes" />

<xsl:template match="Department">
    <Department>
    <Title> <xsl:value-of select="Title" /> </Title>
    <xsl:for-each select="Chair">
        <xsl:copy-of select="." />
    </xsl:for-each>
    </Department>
</xsl:template>

</xsl:stylesheet>
```

# XML Course-Catalog XSLT Exercises Extras

## Q1

Return all courses with enrollment greater than 500. Retain the structure of Course elements from the original data.

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<xsl:stylesheet version="2.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
<xsl:output method="xml" indent="yes" omit-xml-declaration="yes" />

<xsl:template match="Course[@Enrollment &gt; 500]">
	<xsl:copy-of select="." />
</xsl:template>

<xsl:template match="text()" />

</xsl:stylesheet>
```

## Q2

Remove from the data all courses with enrollment greater than 60, or with no enrollment listed. Otherwise the structure of the data should be the same.

```xml
<xsl:stylesheet version="2.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
<xsl:output method="xml" indent="yes" omit-xml-declaration="yes" />

<xsl:template match="*|@*|text()">
   <xsl:copy>
      <xsl:apply-templates select="*|@*|text()" />
   </xsl:copy>
</xsl:template>

<xsl:template match="Course[@Enrollment &gt; 60]" />

<xsl:template match="Course[not(@Enrollment)]" />

</xsl:stylesheet>
```

## Q3

Create a summarized version of the EE part of the course catalog. For each course in EE, return a Course element, with its Number and Title as attributes, its Description as a subelement, and the last name of each instructor as an Instructor subelement. Discard all information about department titles, chairs, enrollment, and prerequisites, as well as all courses in departments other than EE. (Note: To specify quotes within an already-quoted XPath expression, use quot;.)

```xml
<xsl:stylesheet version="2.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
<xsl:output method="xml" indent="yes" omit-xml-declaration="yes" />

<xsl:template match="Department[@Code = 'EE']">
    <xsl:for-each select="Course">
        <Course Number="{@Number}" Title="{Title}">
            <Description> <xsl:value-of select="Description" /> </Description>
            <xsl:for-each select="Instructors/Professor">
                <Instructor> <xsl:value-of select="Last_Name" /> </Instructor>
            </xsl:for-each>
        </Course>
    </xsl:for-each>
</xsl:template>

<xsl:template match="text()" />

</xsl:stylesheet>
```

## Q4

Create an HTML table with one-pixel border that lists all CS department courses with enrollment greater than 200. Each row should contain three cells: the course number in italics, course title in bold, and enrollment. Sort the rows alphabetically by course title. No header is needed. (Note: For formatting, just use "table border=1", and "<b>" and "<i>" tags for bold and italics respectively. To specify quotes within an already-quoted XPath expression, use quot;.)

```xml
<xsl:stylesheet version="2.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
<xsl:output method="html" indent="yes" />

<xsl:template match="Department[@Code = 'CS']">
   <table border="1">
      <xsl:for-each select="Course[@Enrollment &gt; 200]">
      <xsl:sort select="Title" />
        <tr>
            <td><i><xsl:value-of select="@Number" /></i></td>
            <td><b><xsl:value-of select="Title" /></b></td>
            <td><xsl:value-of select="@Enrollment" /></td>
        </tr>
      </xsl:for-each>
   </table>
</xsl:template>

<xsl:template match="text()" />

</xsl:stylesheet>
```