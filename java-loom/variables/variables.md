# Variables in LiveLabs

## Introduction


## Task 1:

Add the following to your manifest.json in the top section:

```
   "variables": ["../../variables/variables.json",
                 "../../variables/variables-in-another-file.json"],
```

## Task 2

Specify the variables in the .json file like this:

*Example: variables.json*
```
{
    "var1": "Variable 1 value",
    "java_version": "Variable 2 value",
    "random_name": "LiveLabs rocks!"
}
 ```

## Task 3

Now, you can refer to those variables using the following syntax (**Please note that you can see the syntax only in markdown**):

[](var:var1)

or

[](var:java_version)

or 

[](var:more_info)



### Examples

(Check the markdown to see the syntax - the bold text is the value of the variable)

- Do you know math? This is **[](var:magic)**

- How many OCPUs do I need to run this service in my paid tenancy? You need **[](var:number_of_ocpu_paid)**

- But what if am using 'Always free'? Then you need **[](var:number_of_ocpu_always_free)**

- What is the best name for my database? It is **[](var:name_of_database)**

- Here you can find more info: **[](var:doc_link)**
