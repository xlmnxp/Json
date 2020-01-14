# Json
Json Deserializer for Alusus Language

# How to Import
```
import "Srl/Console.alusus"
import "Apm.alusus"
Apm.importFile("xlmnxp/Json");

def jsonObject: Json = "{\"glossary\": {\"title\": \"example glossary\",\"GlossDiv\": {\"title\": \"S\", \"GlossList\": {\"GlossEntry\": {\"ID\": \"SGML\",	\"SortAs\": \"SGML\", \"GlossTerm\": \"Standard Generalized Markup Language\", \"Acronym\": \"SGML\", \"Abbrev\": \"ISO 8879:1986\", \"GlossDef\": {\"para\": \"A meta-markup language, used to create markup languages such as DocBook.\",\"GlossSeeAlso\": [\"GML\", \"XML\"]}, \"GlossSee\": \"markup\"}}}}}";
Srl.Console.print(jsonObject.getObject("glossary").getObject("GlossDiv").getObject("GlossList").getObject("GlossEntry").getObject("GlossDef").getObject("GlossSeeAlso").getString(1)); // output: XML
```

# Handlers
- Json~**init(str: ptr[Array[Char]])**<br>
```
initialize Type with parse JSON from String
```

# Methods
- Json.**get(key: ptr[Array[Char]])**: String<br>
```
return value of key as String with Quotations
```
- Json.**get(key: Int)**: String<br>
```
return value of index as String with Quotations
```

- Json.**getString(key: ptr[Array[Char]])**: String<br>
```
return value of key as String without Quotations
```
- Json.**getString(key: Int)**: String<br>
```
return value of index as String without Quotations
```

- Json.**getInt(key: ptr[Array[Char]])**: Int<br>
```
return value of key as Int
```
- Json.**getInt(key: Int)**: Int<br>
```
return value of index as Int
```

- Json.**getFloat(key: ptr[Array[Char]])**: Float<br>
```
return value of key as Float
```
- Json.**getFloat(key: Int)**: Float<br>
```
return value of index as Float
```

- Json.**getBool(key: ptr[Array[Char]])**: Bool<br>
```
return value of key as Boolean
```
- Json.**getBool(key: Int)**: Bool<br>
```
return value of index as Boolean
```

- Json.**getObject(key: ptr[Array[Char]])**: Json<br>
```
return value of key as new Json
```
- Json.**getObject(key: Int)**: Json<br>
```
return value of index as new Json
```

- Json.**isObject()**: Bool<br>
```
return true only when json is Object
```
- Json.**isArray()**: Bool<br>
```
return true only when json is Array
```

- Json.**hasQuotes(stringWithQuotes: String)**: Bool<br>

# Operators
- **this** = **ptr[array[Char]]**]
```
def jsonObject: Json = "{lang: 'alusus'}"
```
- **this** = **String**
```
def str: String = "{lang: 'alusus'}"
def jsonObject: Json = str
```

# LICENSE
MIT LICENSE.