# Q1
In this question, you are to create a DTD for a small XML data set drawn from the Stanford course catalog. There are multiple departments, each with a department chair, some courses, and professors and/or lecturers who teach courses. The XML data is [here](https://prod-c2g.s3.amazonaws.com/db/Winter2013/files/courses-noID.xml).

Write a DTD for the XML data set.

**Important**: Do not include **`<!DOCTYPE Course_Catalog [...]>`** in your DTD. Your DTD should start with **`<!ELEMENT Course_Catalog (Department*)>`**.

```bash
xmllint --valid --noout courses-noID.xml
```