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
where $c/@Enrollment >= max(for $cc in doc("courses.xml")//Course return $cc/data(@Enrollment))
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

## Q2

Return course numbers of courses that have the same title as some other course. (Hint: You might want to use the "preceding" and "following" navigation axes for this query, which were not covered in the video or our demo script; they match any preceding or following node, not just siblings.)

```xquery
for $c in doc("courses.xml")//Course[Title = preceding::*/Title
    or Title = following::*/Title ]
return $c/data(@Number)
```

## Q3

Return course numbers of courses taught by an instructor with first name "Daphne" or "Julie".

```xquery
for $c in doc("courses.xml")//Course
where $c/Instructors//First_Name = "Daphne" or $c/Instructors//First_Name = "Julie"
return $c/data(@Number)
```

## Q4

Return the number (count) of courses that have no lecturers as instructors.

```xquery
count(
    for $c in doc("courses.xml")//Course
    return
        if ($c/Instructors[Lecturer]) then
            ()
        else
            $c
)
```

## Q5

Return titles of courses taught by the chair of a department. For this question, you may assume that all professors have distinct last names.

```xquery
for $c in doc("courses.xml")//Course
where $c//Last_Name = (for $chair in doc("courses.xml")//Chair return $chair//Last_Name)
return $c/Title
```

## Q6

Return titles of courses that have both a lecturer and a professor as instructors. Return each title only once.

```xquery
for $c in doc("courses.xml")//Course
where $c/Instructors[Lecturer] and $c/Instructors[Professor]
return $c/Title
```