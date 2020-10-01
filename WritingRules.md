# How To Write ERDDAP Lint Rules

You're the first one here. We haven't quite figured this out yet but here are some ideas...

The rules are comprised of Markdown files in the rules folder, so they are mostly human readable, 
with each rule implemented as a code block. The code block might be missing if a rule is not 
written yet.

In fact this file is a valid rule set, so you could copy it to the rules folder, change the title
at the top and you're good to go.

A rule in the rules file is comprised of a level 1 heading with some textual description and a fenced
javascript block containing a single filter type function. In practice the textual dexcription and
fenced javascript block may be written by different people.

The function receives a dataset or a variable, a dimension, attribute, for which it can return 
trueish if the dataset passes validation, and falsish if it fails. (A falsish value is false, zero,
null or undefined. A truish value is anything not falsish).

Here's something neat: The parameter passed to the function is determined by the name of the function
parameter.

| Parameter Name | Value                                                                           |
|----------------|---------------------------------------------------------------------------------|
| dataset        | Called once with object containing the full dataset metadata                    |
| NC_GLOBAL      | Called once for each NC_GLOBAL attribute                                        |
| variable       | Called once for each variable in the dataset                                    |
| dimension      | Called once for each dimension in the dataset                                   |
| vardim         | Called once for each variable or dimension in the dataset                       |
| station_name   | Called once for each variable or dimension named station_name or having an attribute named station_name, including NC_GLOBAL |
| ioos_category  | Called once for each variable or dimension named ioos_category or having an attribute named ioos_category, including NC_GLOBAL |

(We will provide an editor that can be used to develop a rule set).

# Accepted Datasets
The Accepted Datasets section is optional.

If the Accepted Datasets exists and contains a valid filter function, then only datasets accepted by
that filter will use the rules contained in the file.

Here's a simple filter which accepts only datasets from Irish Marine Institute:

```javascript
(dataset)=>dataset.url.indexOf("marine.ie/")>=0;
```


# Example Lowercase Rule
All variable and dimension names to be lower case
```javascript
(vardim)=>vardim.name === vardim.name.toLowerCase();
```

# Example Long Name Contains Short
The short name must be included in the long_name
```javascript
(long_name)=>{
  let variable = long_name
  return variable.attributes.long_name.value.indexOf(variable.name) >=0;
}
```