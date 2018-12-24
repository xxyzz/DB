# Databases: DB2 XML Data

Well-formed XML [XML File](https://lagunita.stanford.edu/assets/courseware/v1/027cadb33322f87f38d03efa7479a16e/c4x/DB/XML/asset/Bookstore-noDTD.xml)

DTDs, IDs, and IDREFs [XML File with DTD](https://lagunita.stanford.edu/assets/courseware/v1/fc9416f679c2bd17464ce875603dd9cd/c4x/DB/XML/asset/Bookstore-DTD.xml), [XML File with IDREFs](https://lagunita.stanford.edu/assets/courseware/v1/addb582d3626f32a8d1e83b2baad95fa/c4x/DB/XML/asset/Bookstore-IDREFs.xml)

XML Schema [XML File](https://lagunita.stanford.edu/assets/courseware/v1/ac0c7c927a7ee908458c1992c9635c16/c4x/DB/XML/asset/Bookstore-XSD.xml), [Schema](https://lagunita.stanford.edu/assets/courseware/v1/4085ff683240a62127e7b0bd3d18ae3c/c4x/DB/XML/asset/Bookstore.xsd)

## Q1
In this question, you are to create a DTD for a small XML data set drawn from the Stanford course catalog. There are multiple departments, each with a department chair, some courses, and professors and/or lecturers who teach courses. The XML data is [here](https://prod-c2g.s3.amazonaws.com/db/Winter2013/files/courses-noID.xml).

Write a DTD for the XML data set.

**Important**: Do not include **`<!DOCTYPE Course_Catalog [...]>`** in your DTD. Your DTD should start with **`<!ELEMENT Course_Catalog (Department*)>`**.

```bash
xmllint --valid --noout courses-noID.xml
```

## Q2
In this question, you are to create a DTD for a different version of the data set drawn from the Stanford course catalog. This version encodes the data using ID and IDREF(S) attributes. The XML data is [here](https://prod-c2g.s3.amazonaws.com/db/Winter2013/files/courses-ID.xml).

Write a DTD for the XML data set.

Hint: You may want to use your DTD from the previous question as a starting point, since the structure is similar.

```bash
xmllint --valid --noout courses-ID.xml
```

## Q3
In this question, you are to create a DTD for a small XML data set about world countries. This data is adapted from the Mondial 3.0 database as hosted by [the University of Washington](http://www.cs.washington.edu/research/xmldatasets/www/repository.html#mondial), and was originally compiled by the Georg-August University of [Goettingen Institute for Informatics](http://www.dbis.informatik.uni-goettingen.de/Mondial/). Each country has a name, population, and area (in sq. km). Some countries also list languages (with percentages of the population that speaks each language) and/or cities (with names and populations). The XML data is [here](https://prod-c2g.s3.amazonaws.com/db/Winter2013/files/countries.xml).

```bash
xmllint --valid --noout countries.xml
```