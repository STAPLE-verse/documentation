# Annotations

<https://www.xtivia.com/blog/using-react-jsonschema-form-with-tailwind-css-and-daisyui/>

-   title: short description
-   description: longer description
-   default: shows a default value for the object
-   examples: shows examples, will include the default as an example
-   readOnly: a boolean (true false) that shows it should only be read in
-   writeOnly: indicates that a value may be set, but will remain hidden
-   required: should be an array of names of objects
-   type: the type of object you want to create
    -   object: a dictionary that can have nested properties

        -   properties: dictionary of key value pairs (nesting schema)
        -   additionalProperties: do you allow additional stuff to be added to this dictionary? (boolean)
        -   patternProperties: regular expressions for the types of properties that can be added
        -   unevaluatedProperties: similar to additionalProperties, a boolean to have unevaluated properties
        -   required: which properties are required within this object
        -   propertyNames: ways to validate the names of properties
        -   minProperties
        -   maxProperties

    -   string

        -   maxLength:
        -   minLength:
        -   format: The format keyword allows for basic semantic identification of certain kinds of string values that are commonly used.
        -   https://json-schema.org/understanding-json-schema/reference/string#built-in-formats
        -   pattern: allows you to use regex or other matching to determine what the input should look like, validation

    -   number/integer

        -   multipleOf: can be a whole number or float point
        -   x ≥ minimum
        -   x \> exclusiveMinimum
        -   x ≤ maximum
        -   x \< exclusiveMaximum

    -   array

        -   items: a dictionary of properties/items or a true/false denoting that other items beyond prefixItems can be added - all items must be validated

        -   prefixItems: a dictionary of properties/items

        -   unevaluatedItems: a true/false that we should allow unevaluated items

        -   contains: a dictionary of properties/items, only one item must be validated

        -   minContains

        -   maxContains

        -   minItems

        -   maxItems

        -   uniqueItems: true/false denoting if all items must be unique

    -   boolean

    -   null

    -   https://json-schema.org/understanding-json-schema/reference/type
-   const: restricts the input to a single value, this exact pattern match
    -   "const": "United States of America"
-   enum: an array of allowable exact values
    -   "enum": \["red", "amber", "green"\]

# Notes from React Form

```         
{
  "definitions": {
    "Thing": {
      "type": "object",
      "properties": {
        "name": {
          "type": "string",
          "default": "Default name"
        }
      }
    }
  },
  "type": "object",
  "properties": {
    "listOfStrings": {
      "type": "array",
      "title": "A list of strings",
      "items": {
        "type": "string",
        "default": "bazinga"
      }
    },
    "multipleChoicesList": {
      "type": "array",
      "title": "A multiple choices list",
      "items": {
        "type": "string",
        "enum": [
          "foo",
          "bar",
          "fuzz",
          "qux"
        ]
      },
      "uniqueItems": true
    },
    "fixedItemsList": {
      "type": "array",
      "title": "A list of fixed items",
      "items": [
        {
          "title": "A string value",
          "type": "string",
          "default": "lorem ipsum"
        },
        {
          "title": "a boolean value",
          "type": "boolean"
        }
      ],
      "additionalItems": {
        "title": "Additional item",
        "type": "number"
      }
    },
    "minItemsList": {
      "type": "array",
      "title": "A list with a minimal number of items",
      "minItems": 3,
      "items": {
        "$ref": "#/definitions/Thing"
      }
    },
    "defaultsAndMinItems": {
      "type": "array",
      "title": "List and item level defaults",
      "minItems": 5,
      "default": [
        "carp",
        "trout",
        "bream"
      ],
      "items": {
        "type": "string",
        "default": "unidentified"
      }
    },
    "nestedList": {
      "type": "array",
      "title": "Nested list",
      "items": {
        "type": "array",
        "title": "Inner list",
        "items": {
          "type": "string",
          "default": "lorem ipsum"
        }
      }
    },
    "unorderable": {
      "title": "Unorderable items",
      "type": "array",
      "items": {
        "type": "string",
        "default": "lorem ipsum"
      }
    },
    "copyable": {
      "title": "Copyable items",
      "type": "array",
      "items": {
        "type": "string",
        "default": "lorem ipsum"
      }
    },
    "unremovable": {
      "title": "Unremovable items",
      "type": "array",
      "items": {
        "type": "string",
        "default": "lorem ipsum"
      }
    },
    "noToolbar": {
      "title": "No add, remove and order buttons",
      "type": "array",
      "items": {
        "type": "string",
        "default": "lorem ipsum"
      }
    },
    "fixedNoToolbar": {
      "title": "Fixed array without buttons",
      "type": "array",
      "items": [
        {
          "title": "A number",
          "type": "number",
          "default": 42
        },
        {
          "title": "A boolean",
          "type": "boolean",
          "default": false
        }
      ],
      "additionalItems": {
        "title": "A string",
        "type": "string",
        "default": "lorem ipsum"
      }
    }
  }
}
```

```         
{
  "title": "Widgets",
  "type": "object",
  "properties": {
    "stringFormats": {
      "type": "object",
      "title": "String formats",
      "properties": {
        "email": {
          "type": "string",
          "format": "email"
        },
        "uri": {
          "type": "string",
          "format": "uri"
        }
      }
    },
    "boolean": {
      "type": "object",
      "title": "Boolean field",
      "properties": {
        "default": {
          "type": "boolean",
          "title": "checkbox (default)",
          "description": "This is the checkbox-description"
        },
        "radio": {
          "type": "boolean",
          "title": "radio buttons",
          "description": "This is the radio-description"
        },
        "select": {
          "type": "boolean",
          "title": "select box",
          "description": "This is the select-description"
        }
      }
    },
    "string": {
      "type": "object",
      "title": "String field",
      "properties": {
        "default": {
          "type": "string",
          "title": "text input (default)"
        },
        "textarea": {
          "type": "string",
          "title": "textarea"
        },
        "placeholder": {
          "type": "string"
        },
        "color": {
          "type": "string",
          "title": "color picker",
          "default": "#151ce6"
        }
      }
    },
    "secret": {
      "type": "string",
      "default": "I'm a hidden string."
    },
    "disabled": {
      "type": "string",
      "title": "A disabled field",
      "default": "I am disabled."
    },
    "readonly": {
      "type": "string",
      "title": "A readonly field",
      "default": "I am read-only."
    },
    "readonly2": {
      "type": "string",
      "title": "Another readonly field",
      "default": "I am also read-only.",
      "readOnly": true
    },
    "widgetOptions": {
      "title": "Custom widget with options",
      "type": "string",
      "default": "I am yellow"
    },
    "selectWidgetOptions": {
      "title": "Custom select widget with options",
      "type": "string",
      "enum": [
        "foo",
        "bar"
      ]
    },
    "selectWidgetOptions2": {
      "title": "Custom select widget with options, overriding the enum titles.",
      "type": "string",
      "oneOf": [
        {
          "const": "foo",
          "title": "Foo"
        },
        {
          "const": "bar",
          "title": "Bar"
        }
      ]
    }
  }
}
```

```         
{
  "definitions": {
    "largeEnum": {
      "type": "string",
      "enum": [
        "option #0",
        "option #1",
        "option #2",
        "option #3",
        "option #4",
        "option #5",
        "option #6",
        "option #7",
        "option #8",
        "option #9",
        "option #10",
        "option #11",
        "option #12",
        "option #13",
        "option #14",
        "option #15",
        "option #16",
        "option #17",
        "option #18",
        "option #19",
        "option #20",
        "option #21",
        "option #22",
        "option #23",
        "option #24",
        "option #25",
        "option #26",
        "option #27",
        "option #28",
        "option #29",
        "option #30",
        "option #31",
        "option #32",
        "option #33",
        "option #34",
        "option #35",
        "option #36",
        "option #37",
        "option #38",
        "option #39",
        "option #40",
        "option #41",
        "option #42",
        "option #43",
        "option #44",
        "option #45",
        "option #46",
        "option #47",
        "option #48",
        "option #49",
        "option #50",
        "option #51",
        "option #52",
        "option #53",
        "option #54",
        "option #55",
        "option #56",
        "option #57",
        "option #58",
        "option #59",
        "option #60",
        "option #61",
        "option #62",
        "option #63",
        "option #64",
        "option #65",
        "option #66",
        "option #67",
        "option #68",
        "option #69",
        "option #70",
        "option #71",
        "option #72",
        "option #73",
        "option #74",
        "option #75",
        "option #76",
        "option #77",
        "option #78",
        "option #79",
        "option #80",
        "option #81",
        "option #82",
        "option #83",
        "option #84",
        "option #85",
        "option #86",
        "option #87",
        "option #88",
        "option #89",
        "option #90",
        "option #91",
        "option #92",
        "option #93",
        "option #94",
        "option #95",
        "option #96",
        "option #97",
        "option #98",
        "option #99"
      ]
    }
  },
  "title": "A rather large form",
  "type": "object",
  "properties": {
    "string": {
      "type": "string",
      "title": "Some string"
    },
    "choice1": {
      "$ref": "#/definitions/largeEnum"
    },
    "choice2": {
      "$ref": "#/definitions/largeEnum"
    },
    "choice3": {
      "$ref": "#/definitions/largeEnum"
    },
    "choice4": {
      "$ref": "#/definitions/largeEnum"
    },
    "choice5": {
      "$ref": "#/definitions/largeEnum"
    },
    "choice6": {
      "$ref": "#/definitions/largeEnum"
    },
    "choice7": {
      "$ref": "#/definitions/largeEnum"
    },
    "choice8": {
      "$ref": "#/definitions/largeEnum"
    },
    "choice9": {
      "$ref": "#/definitions/largeEnum"
    },
    "choice10": {
      "$ref": "#/definitions/largeEnum"
    }
  }
}
```

```         
{
  "title": "Date and time widgets",
  "type": "object",
  "properties": {
    "native": {
      "title": "Native",
      "description": "May not work on some browsers, notably Firefox Desktop and IE.",
      "type": "object",
      "properties": {
        "datetime": {
          "type": "string",
          "format": "date-time"
        },
        "date": {
          "type": "string",
          "format": "date"
        },
        "time": {
          "type": "string",
          "format": "time"
        }
      }
    },
    "alternative": {
      "title": "Alternative",
      "description": "These work on most platforms.",
      "type": "object",
      "properties": {
        "alt-datetime": {
          "type": "string",
          "format": "date-time"
        },
        "alt-date": {
          "type": "string",
          "format": "date"
        }
      }
    }
  }
}
```

```         
{
  "definitions": {
    "locations": {
      "enumNames": [
        "New York",
        "Amsterdam",
        "Hong Kong"
      ],
      "enum": [
        {
          "name": "New York",
          "lat": 40,
          "lon": 74
        },
        {
          "name": "Amsterdam",
          "lat": 52,
          "lon": 5
        },
        {
          "name": "Hong Kong",
          "lat": 22,
          "lon": 114
        }
      ]
    }
  },
  "type": "object",
  "properties": {
    "location": {
      "title": "Location",
      "$ref": "#/definitions/locations"
    },
    "locationRadio": {
      "title": "Location Radio",
      "$ref": "#/definitions/locations"
    },
    "multiSelect": {
      "title": "Locations",
      "type": "array",
      "uniqueItems": true,
      "items": {
        "$ref": "#/definitions/locations"
      }
    },
    "checkboxes": {
      "title": "Locations Checkboxes",
      "type": "array",
      "uniqueItems": true,
      "items": {
        "$ref": "#/definitions/locations"
      }
    }
  }
}
```
