# How it works
The XML processing involves two main steps: expanding nodes and replacing placeholders.

1. Expanding Nodes
XML nodes like `<forces>...</forces>`, `<units>...</units>`, and `<profiles>...</profiles>` are expanded by replacing them with their child elements. This process is repeated for each of the matching objects, effectively generating multiple sets of the child elements.
For example: a roster with 2 units with this XML:
```xml
<units>
  <div>Unit</div>
  <div>{{name}}</div>
</units>
```
Will be expanded to
```xml
<div>Unit</div>
<div>{{name}}</div>
<div>Unit</div>
<div>{{name}}</div>
```

2. Replacing Placeholders
Within XML nodes, any text wrapped in {{...}} will be replaced with the corresponding value. For example:
```xml
<profiles>
  <span>{{name}}</span>
</profiles>
```
Results in (for each matching profile):
```html
<span>Example</span>
```

# Common
All elements have these fields/values.  
Values: `name`,
`first`: Indicates if this is the first element in a list  
`last`: Indicates if this is the last element in a list  
`odd`: Indicates if this is an odd element in a list  
`even`: Indicates if this is an even element in a list  
`length`: The length of the containing list  
`index`: The (zero-based) position of the element in its containing list  

# `<forces>`
Values `options`, `hidden`  
Lists: `units`, `entries`, `costs`  
# `<units>`
Values:`primary`, `options`, `secondaries`, `type`, `hidden`, `amount`  
Lists: `entries`, `costs`, `profiles`, `rules`, `secondaries`  
# `<entries>`
Values: `primary`, `options`, `secondaries`, `type`, `hidden`, `amount`  
Lists: `entries`, `costs`, `profiles`, `rules`, `secondaries`  
# `<profiles>`
Values:  `hidden`, `typeName`, `typeId`, `page`, `group`,  
Lists: `characteristics`  
# `<characteristics>`
Values: `value`, `originalValue`, `typeId`  
# `<rules>`
Values: `description`, `hidden`, `page`  
# `<costs>`
Values: `value`, `typeId`  
# `<secondaries>`


# `<if>`
This node and its child will be not be rendered if it does not match a condition  
Types:
`equals`: `field` is equal to `value`  
`not-equals`: `field` is not equal to `value`  
`match`: `field` matches the regex in `value`; flags can be added in `flags`  
`not-match`:  `field` does not match the regex in `value`; flags can be added in `flags`  
`has-profile`:  `field` (or the current element) has a profile with typeName in `value` (case insensitive, separate by `,`)  
`has-not-profile`:  `field` (or the current element) does not have a profile with typeName in `value` (case insensitive, separate by `,`)  
`not`: `field` is falsy  
(no type): `field` is truthy

If the field is `length`, these types are also supported: `greater`, `atleast`, `less`, `atmost`, `equals`, `not-equals`

Example:
```xml
<entries>
  <if type="has-profile" value="model,stats">
    ...
  </if>
</entries>
```



