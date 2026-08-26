# Json

[[عربي]](README.ar.md)

A JSON library for Alusus Language. It supports reading and extracting information from JSON, serializing
objects to JSON, and parsing JSON directly into typed objects.

## Adding to the Project

You can add it to the project using APM:

```
import "Apm";
Apm.importPackage("Alusus/Json@0.2");
```

## Example

```
import "Srl/Console.alusus"
import "Apm";
Apm.importPackage("Alusus/Json@0.2");

use Srl
func start {
    // define a variable with type Json to hold the information we want.
    def obj: Json = "{\n\t\"id\": 1,\n\t\"classes\": [\"c1\", \"c2\", \"c3\"],\n\t\"passed\": true\n}"

    // get the value stored under the key `id` as integer variable.
    def id: int = obj("id");
    Console.print("id is %d\n", id);

    // check if the value stored under the key `classes` is an array.
    def classesObj: Json = obj("classes");
    if (classesObj.isArray()) {
        Console.print("Array\n");
    }
    else {
        Console.print("Not array\n");
    }

    // get the value stored under the key `passed` as boolean variable
    def passed: Bool = obj("passed");
    // check if the student passed the exam or not.
    if (passed) {
        Console.print("passed\n");
    }
    else {
        Console.print("failed\n");
    }

}
start();
```

## Json class

### Member Variables

```
class Json {
    def keys: Array[String];
    def values: Array[Json];
    def rawValue: String;
}
```

`keys`: keys of the JSON if it's an object.

`values`: values of the JSON if it's an array or an object.

`rawValue`: raw value (unparsed) if the JSON is neither an object nor an array.

### Initialization

We can initialize this class using many methods:

```
handler this~init()
```
default initialization without parameters.

```
handler this~init(str: ptr[array[Char]])
```
initialization from an array of char.

```
handler this~init(json: ref[Json])
```
initialization from another object of the class, copy initialization.

### isObject

```
handler this.isObject(): bool;
```
Checks if the JSON is an object.

### isArray

```
handler this.isArray(): bool;
```
Checks if the JSON is an array or not.

### getLength

```
handler this.getLength(): Int;
```
Get the number of values in the JSON. This will return a positive number if the JSON is an object
or an array.

### getKey

```
handler this.getKey(index: Int): String;
```
Returns the key at at the specified index if the JSON is an object. Returns an empty string if
the JSON is not an object or the index is out of range.

### stringfy

```
func stringfy [T: type] (obj: ref[T]): String;
```

Serializes an object's fields to a JSON string. Supported field types are `Nullable[T]`,
`Array[Nullable[T]]`, `Map[String, Nullable[T]]`, `Map[String, Array[Nullable[T]]]`, and
`Array[Map[String, Nullable[T]]]`, where `T` is `String`, `Bool`, `Int[32]`, `Int[64]`, `Float[32]`,
or `Float[64]`. An unset `Nullable` value is serialized as `null`.

```
class Person {
    def name: Nullable[String];
    def tags: Array[Nullable[String]];
}

def person: Person();
person.name = Nullable[String](String("Sarmed"));
person.tags.add(Nullable[String](String("admin")));

Console.print("%s\n", Json.stringfy[Person](person).buf);
// Output: {"name":"Sarmed","tags":["admin"]}
```

### parse

```
func parse [T: type] (obj: ref[T], str: CharsPtr);
```

Populates an existing object's fields from a JSON string, matching each JSON key against the
object's field names. Supports the same field types as `stringfy`.

```
def person: Person();
Json.parse[Person](person, "{\"name\": \"Sarmed\", \"tags\": [\"admin\"]}");
Console.print("%s\n", person.name.value.buf);
// Output: Sarmed
```

## JsonStringBuilderMixin

The `JsonStringBuilderMixin` is a mixin that can be used with `StringBuilder` to provide JSON string
formatting capabilities. It automatically escapes special characters in strings when building JSON output.

### Usage

```
import "Srl/StringBuilder";
import "Apm";
Apm.importPackage("Alusus/Json@0.2");
use Srl;

def stringBuilder: StringBuilder[JsonStringBuilderMixin](512, 512);
```

### Format Specifiers

The mixin provides special format specifiers for the `format` method:

- `%js` - Format a `String` value with proper JSON escaping (escapes quotes, backslashes, etc.)
- `%jpc` - Format a `CharsPtr` value with proper JSON escaping
- `%jns` - Format a `Nullable[String]` value, writing `null` if it has no value, otherwise applying the same JSON escaping as `%js`

### Example

```
import "Srl/Console";
import "Srl/StringBuilder";
import "Srl/Nullable";
import "Apm";
Apm.importPackage("Alusus/Json@0.2");
use Srl;

func testJsonStringBuilderMixin {
    def stringBuilder: StringBuilder[JsonStringBuilderMixin](512, 512);
    def stringVal: String("test\"quotes'");
    def charsPtrVal: CharsPtr("test\"quotes'");
    def nullableVal: Nullable[String]("test\"quotes'");
    def nullVal: Nullable[String];
    stringBuilder.format(
        "{ \"name\": %js, \"value\": %jpc, \"nullable\": %jns, \"null\": %jns }",
        stringVal, charsPtrVal, nullableVal, nullVal
    );
    Console.print("%s\n", stringBuilder~cast[String].buf);
}
testJsonStringBuilderMixin();
```

Output:
```
{ "name": "test\"quotes'", "value": "test\"quotes'", "nullable": "test\"quotes'", "null": null }
```

---

## License

This project is licensed under the MIT license. See the `LICENSE` file for details.

