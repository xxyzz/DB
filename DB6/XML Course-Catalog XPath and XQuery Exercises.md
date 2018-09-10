# XML Course-Catalog XPath and XQuery Exercises

In these exercises, you will be working with a small XML data set drawn from the Stanford course catalog. There are multiple departments, each with a department chair, some courses, and professors and/or lecturers who teach courses. The XML data is [here](https://prod-c2g.s3.amazonaws.com/db/Winter2013/files/courses-noID.xml).

## Q1

Return all Title elements (of both departments and courses).

```xquery
for $c in doc("courses.xml")//Title
return $c
```

## Q2

Return last names of all department chairs.

```xquery
for $l in doc("courses.xml")//Department/Chair//Last_Name
return $l
```