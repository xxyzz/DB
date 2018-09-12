# XML World-Countries XPath and XQuery Exercises

## Q1

Return the area of Mongolia.

```xquery
doc("countries.xml")//country[@name="Mongolia"]/data(@area)
```

## Q2

Return the names of all cities that have the same name as the country in which they are located.

```xquery
let $countries := doc("countries.xml")//country
let $cities := $countries//city
for $c in $cities
where $c/name = $countries/@name
return $c/name
```

## Q3

Return the average population of Russian-speaking countries.

```xquery
let $population := (for $c in doc("countries.xml")//country
                    where $c//language = "Russian"
                    return $c/data(@population))
return avg($population)
```

## Q4

Return the names of all countries that have at least three cities with population greater than 3 million.

```xquery
for $country in doc("countries.xml")//country
where count($country/city[population > 3000000]) > 3
return $country/data(@name)
```

## Q5

Create a list of French-speaking and German-speaking countries. The result should take the form:
```xml
<result>
  <French>
    <country>country-name</country>
    <country>country-name</country>
    ...
  </French>
  <German>
    <country>country-name</country>
    <country>country-name</country>
    ...
  </German>
</result>
```

```xquery
let $french := doc("countries.xml")//country[language = "French"]/data(@name)
let $german := doc("countries.xml")//country[language = "German"]/data(@name)
return <result>
           <French>
           {
               for $c in $french
               return <country> { $c } </country>
           }
           </French>
           <German>
           {
               for $c in $german
               return <country> { $c } </country>
           }
           </German>
       </result>
```

## Q6

Return the countries with the highest and lowest population densities. Note that because the "/" operator has its own meaning in XPath and XQuery, the division operator is infix "div". To compute population density use "(@population div @area)". You can assume density values are unique. The result should take the form:

```xml
<result>
  <highest density="value">country-name</highest>
  <lowest density="value">country-name</lowest>
</result>
```

```xquery
let $countries := doc("countries.xml")//country
let $hi := (for $c in $countries
            where $c/data(@population div @area) = max($countries/data(@population div @area))
            return $c)
let $lo := (for $c in $countries
            where $c/data(@population div @area) = min($countries/data(@population div @area))
            return $c)
return 
    <result>
        <highest density = "{ $hi/data(@population div @area) }"> { $hi/data(@name) } </highest>
        <lowest density = "{ $lo/data(@population div @area) }"> { $lo/data(@name) } </lowest>
    </result>
```