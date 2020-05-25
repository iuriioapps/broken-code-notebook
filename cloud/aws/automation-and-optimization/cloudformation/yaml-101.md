# YAML 101

### Key/value pairs

* YAML document consists of key/value pairs
* Key/value pairs are separated by colon followed by space
* Supports different data types
  * Integers
  * Floating-point numbers
  * Strings, enclosed with quotes if they have special symbols
  * Boolean
  * Binary
  * Dates, in ISO:8601 format
  * Null value

```yaml
Name: Dave
Age: 25
GPA: 4.2
Occupation: Engineer
State: 'New Jersey'
About: "I'm a software engineer"
Score: null
Male: true
DateOfBirth: 1990-09-15T19:07:00
```

### Lists

* YAML lists are indented with a dash
* All items are on the same level
* There are 2 types of writing arrays:
  * Block sequence - each entry indicated with a dash followed by a space
  * Flow sequence - all elements are on the same line, separated with commas and enclosed by \[\].

```yaml
PeopleBlock:
  - John
  - Mike
  - Tom
  - Jake

PeopleFlow: [John, Mike, Tom, Jake]
```

### Dictionaries

* A set of properties grouped under an item
* Dictionaries contain key/value pairs

```yaml
Dave:
  Age: 25
  GPA: 4.2
  Occupation: Engineer
  State: 'New Jersey'
  About: "I'm a software engineer"
  Score: null
  Male: true
  DateOfBirth: 1990-09-15T19:07:00
```

### Lists with Dictionaries

```yaml
Personnel:
  - Dave:
      Age: 24
      Occupation: Engineer
      State: CA
  - Mike:
      Age: 30
      Occupation: Manager
      State: TX
  - John:
      Age: 40
      Occupation: Designer
      State: WA
```

### Lists with Dictionaries containing Lists

```yaml
Personnel:
  - Dave:
      Age: 24
      Occupation: Engineer
      State: CA
      Degrees:
      	- Bachelor
      	- Masters
      	- PHD
  - Mike:
      Age: 30
      Occupation: Manager
      State: TX
      Degrees: [Bachelor, Masters]
  - John:
      Age: 40
      Occupation: Designer
      State: WA
      Degrees:
      	- Masters
```

### Pipes

* Pipe notation is also referred as literal block
* All new lines, indentation, extra spaces are preserved

```yaml
Resource:
  Name: My resource
  Description: |
    This is a description,
    with new lines and | special symbols
  Prop: value
```

### Greater than

* Also referred as folded block
* Renders the text as a single line
* All new lines will be replaced with a single space
* Blank lines are converted to a new line character

```yaml
Resource:
  Name: My resource
  Description: >
    This is a description,
    with new lines and | special symbols

    Will render everything as a single line
  Prop: value
```

### Comments

* Comments are defined with '\#' symbol

```yaml
# resource definition
Resource:
  Name: My resource
```

### Anchor / merge

* Allows to define and reference blocks
* CloudFormation has very limited support for anchors 

```yaml
MyAnchor: &a
  Prop1: value1
  Prop2: value2

AnotherPlace:
  <<: *a
  Prop3: value3
```



