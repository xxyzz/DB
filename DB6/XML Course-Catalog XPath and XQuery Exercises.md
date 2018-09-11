# XML Course-Catalog XPath and XQuery Exercises

In these exercises, you will be working with a small XML data set drawn from the Stanford course catalog. There are multiple departments, each with a department chair, some courses, and professors and/or lecturers who teach courses. The XML data is [here](https://prod-c2g.s3.amazonaws.com/db/Winter2013/files/courses-noID.xml).

[Download Kernow](https://sourceforge.net/projects/kernowforsaxon/files/latest/download)

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

## Q3

Return titles of courses with enrollment greater than 500.

```xquery
for $c in doc("courses.xml")//Course
where $c/@Enrollment > 500
return $c/Title
```

## Q4

Return titles of departments that have some course that takes "CS106B" as a prerequisite.

```xquery
for $d in doc("courses.xml")//Department
where $d/Course//Prereq = "CS106B"
return $d/Title
```

## Q5

Return last names of all professors or lecturers who use a middle initial. Don't worry about eliminating duplicates.

```xquery
for $pl in doc("courses.xml")//(Professor | Lecturer)
where $pl[Middle_Initial]
return $pl/Last_Name
```

## Q6

Return the count of courses that have a cross-listed course (i.e., that have "Cross-listed" in their description).

```xquery
count(doc("courses.xml")//Course[contains(Description, "Cross-listed")])
```

## Q7

Return the average enrollment of all courses in the CS department.

```xquery
for $d in doc("courses.xml")//Department
where $d/@Code = "CS"
return avg($d/Course/@Enrollment)
```

## Q8

Return last names of instructors teaching at least one course that has "system" in its description and enrollment greater than 100.

```xquery
for $c in doc("courses.xml")//Course
where $c[contains(Description, "system")]
    and $c/@Enrollment > 100
return $c//Last_Name
```

## Q9

Return the title of the course with the largest enrollment.

```xquery
for $c in doc("courses.xml")//Course
where $c/@Enrollment >= max(for $cc in doc("courses.xml")//Course return $cc/@Enrollment)
return $c/Title
```

# XML Course-Catalog XPath and XQuery Exercises Extras

## Q1

Return the course number of the course that is cross-listed as "LING180".

```xquery
for $c in doc("courses.xml")//Course
where $c[contains(Description, "Cross-listed as LING180")]
return $c/data(@Number)
```