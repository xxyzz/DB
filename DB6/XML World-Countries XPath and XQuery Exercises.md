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

# XML World-Countries XPath and XQuery Exercises Extras

## Q1

Return the names of all countries with population greater than 100 million.

```xquery
for $c in doc("countries.xml")//country
where $c/@population > 100000000
return $c/data(@name)
```

## Q2

Return the names of all countries where over 50% of the population speaks German. (Hint: Depending on your solution, you may want to use ".", which refers to the "current element" within an XPath expression.)

```xquery
for $language in doc("countries.xml")//language
where $language = "German" and $language/@percentage > 50
return $language/parent::country/data(@name)
```

## Q3

Return the names of all countries where a city in that country contains more than one-third of the country's population.

```xquery
let $countries := doc("countries.xml")//country
for $country in $countries
for $city in $country/city
where $city/population * 3 > $country/@population
return $country/data(@name)
```

## Q4

Return the population density of Qatar. Note: Since the "/" operator has its own meaning in XPath and XQuery, the division operator is "div". To compute population density use "(@population div @area)".

```xquery
let $q := doc("countries.xml")//country[@name = "Qatar"]
return $q/data(@population) div $q/data(@area)
```

## Q5

Return the names of all countries whose population is less than one thousandth that of some city (in any country).

```xquery
let $counries := doc("countries.xml")//country
let $cities := doc("countries.xml")//city
for $country in $countires
for $city in $cities
where $contry/@population + 1000 < $city/population
return $country/data(@name)
```

## Q6

Return all city names that appear more than once, i.e., there is more than one city with that name in the data. Return only one instance of each such city name. (Hint: You might want to use the "preceding" and/or "following" navigation axes for this query, which were not covered in the video or our demo script; they match any preceding or following node, not just siblings.)

```xquery
for $c in doc("countries.xml")//city[name = preceding::name]
return $c/name
```

## Q7

Return the names of all countries containing a city such that some other country has a city of the same name. (Hint: You might want to use the "preceding" and/or "following" navigation axes for this query, which were not covered in the video or our demo script; they match any preceding or following node, not just siblings.)

```xquery
for $c in doc("countries.xml")//country
where $c/city/name = $c/preceding::city/name or $c/city/name = $c/following::city/name
return $c/data(@name)
```

## Q8

Return the names of all countries whose name textually contains a language spoken in that country. For instance, Uzbek is spoken in Uzbekistan, so return Uzbekistan. (Hint: You may want to use ".", which refers to the "current element" within an XPath expression.)

```xquery
for $country in doc("countries.xml")//country
where some $language in $country/language satisfies $country[contains(@name, $language)]
return $country/data(@name)
```

## Q9

Return the names of all countries in which people speak a language whose name textually contains the name of the country. For instance, Japanese is spoken in Japan, so return Japan. (Hint: You may want to use ".", which refers to the "current element" within an XPath expression.)

```xquery
for $country in doc("countries.xml")//country
where some $language in $country/language satisfies $language[contains(., $country/@name)]
return $country/data(@name)
```

## Q10

Return all languages spoken in a country whose name textually contains the language name. For instance, German is spoken in Germany, so return German. (Hint: Depending on your solution, may want to use data(.), which returns the text value of the "current element" within an XPath expression.)

```xquery
for $language in doc("countries.xml")//language
where contains($language/parent::country/@name, $language)
return $language/data(.)
```