# JSON (Primer #1)

[home](../README.md) | [section index](README.md)

## 1. The JSON format

`JSON` stands for `JavaScript Object Notation`, in plain English a way to store 
and restore JS objects. Very quickly from language specific, 
and handy tool it became de-facto standard for documents interchanged 
by machines (via APIs) in the Internet. 
Agreed-upon media type for JSON is `application/json` which 
is sent in `HTTP headers` by any JSON enabled API.
Shortly after that followed adoption of JSON as standard 
document format in many `NoSQL` databases including 
`MongoDB`. See [Wikipedia article](https://en.wikipedia.org/wiki/JSON) for more...

Because it is technically just a string **there is a couple 
of problems to do with portability of values** between platforms, 
two most important ones are:

 * **encoding sensitive** (if not explicitly stated by sending and/or not followed by receiving party the content of strings with international characters can be garbled).
 * **basic value types** (following what is implemented natively in Javascript). This problem is (sort-of) addressed by elements like [schema](https://en.wikipedia.org/wiki/JSON#Schema_and_metadata) 
   but it makes the format bulky and contradicts its original light-weight nature.

**Another problem is that being parsed in different languages JSON strings build objects of a structure that may vary from system to system** - one of the examples: `order of keys/properties` pointing at values is described below in more details.

## 2. Validation

#### 2.1. Web Tools

To validate any `JSON` (as string) you can use [JSON Lint](https://jsonlint.com/) - here as web-based validator.

**Fig. A** - Just copy/paste this minified string:

```json
{"string":"This is a string","number":{"integer":1,"real":3.12,"negative":-997},"object":{"is":"another nested structure","having":{"other":"objects too"}},"array":[1,2,3,"random elements",{"objects":"as well"}],"boolean":[true,false],"null":null}
```

**Fig. B** Run `[Validate JSON]` in `JSON Lint` and you should get it validated and turned into human-readable:

```json
{
    "string": "This is a string",
    "number": {
        "integer": 1,
        "real": 3.12,
        "negative": -997
    },
    "object": {
        "is": "another nested structure",
        "having": {
            "other": "objects too"
        }
    },
    "array": [
        1,
        2,
        3,
        "random elements",
        {
            "objects": "as well"
        }
    ],
    "boolean": [true, false],
    "null": null
}
```

Now, copy the above, formatted string, and use [another website](https://www.cleancss.com/json-minify/) to get it back to minified format like it was in **Fig. A**.

#### 2.2. Command-line tools

Install package containing the jsonlint command:

```bash
sudo apt-get install python-demjson
```

and verify correctness of installation by running:

```bash
jsonlint --version
```

***expected***: output similar to one below:

```bash
jsonlint version 1.6 (2011-04-01)
demjson version 1.6 (2011-04-01)
```

**Fig. A** Store JSON from 2.1. above into a file (copy string into clipboard first)

```bash
cat > myjson2validate.json

# then just <Ctrl+V> or paste using other 
# keys combination or mouse
# 
# next click [Enter]
# 
# and finally <Ctrl+D> to exit storing into file
```

Now, verify the file by displaying its contents

```bash
cat myjson2validate.json
```

**expected**: you should see your string you copied into clipboard before...


**Fig. B** Validation of a file containing JSON string from command line

```bash
jsonlint -f myjson2validate.json
```

**expected**: It should display something similar:

```bash
{ "array" : [ 1,
      2,
      3,
      "random elements",
      { "objects" : "as well" }
    ],
  "boolean" : [ true,
      false
    ],
  "null" : null,
  "number" : { "integer" : 1,
      "negative" : -997,
      "real" : 3.12
    },
  "object" : { "having" : { "other" : "objects too" },
      "is" : "another nested structure"
    },
  "string" : "This is a string"
}
```


```
Important to remember: 
```

Although, **displayed object seems to have the same content**, 
please notice that original keys (pointing at values) 
were reshuffled (sorted) so that now the key `array` not `string`
is first in displayed object.
**JSON parsers in different languages** may impose 
different (sometimes strange at first) behaviours. 
Therefore **DO NOT rely on the order of keys inside 
the object** - ie. do not hard-code any checks, etc...

**Fig. C** Validation of a broken JSON string in file

Let's break JSON format in the file first adding additional, needless `curly brace`.

```bash
echo "}" >> myjson2validate.json # we are writing this with >> which is 'append'
```

Now let's test the broken JSON

```bash
jsonlint -f myjson2validate.json
```

**expected**: empty output (no object details as JSON to create object was broken)

Additionally we can run check of the `bash exit code` of the `jsonlint` command like this:

```bash
jsonlint -f myjson2validate.json
echo $?
```
or shorthand... (`;` is for bash a notion of the new line/new command)

```bash
jsonlint -f myjson2validate.json; echo $?
```

**expected**: Value of 1 (which in `POSIX` systems like Mac `OSX`, `Linux` or `BSD` means non-zero, `error code`).

**Fig, D** Fix the file and retest with exit code

Open `myjson2validate.json` in your editor 
and remove last `curly brace` and save file.
Now **repeat the fig. A** above - **same results expected**.

## 3. JSONPath

Schema concept for JSON was borrowed from XML world and likewise the jsonpath is a cousin of the XPath.

* `XPath`: [https://en.wikipedia.org/wiki/XPath](https://en.wikipedia.org/wiki/XPath)
* `JSONPath`: [https://www.npmjs.com/package/JSONPath](https://www.npmjs.com/package/JSONPath) - this link nice comparison of the two

## 4. Javascript (NodeJS et al.)

Most notable use of `JSON` and `MongoDB` over last 
couple of years is building web applications 
enabling so-called RESTful APIs - before we build 
our first API we need to dive into `Javascript`.

Go to - [Lesson #2 - Javascript primer](../lessons/02-Javascript.md)