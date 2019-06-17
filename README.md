## JSON parser for CMake

Module JSONParser.cmake contains macro `sbeParseJson` to parse JSON string stored in given variable (second argument). 
Macro fills given variable (first argument) with list of all names in JSON string. For each name it creates variable and sets the value from JSON string.
To clear created variables use macro `sbeClearJson`.

> **Module does not validate given JSON string. If given JSON is malformed, the macro gives unpredictable result.**

### Example

You can play with test configuration provided in example files `CMakeLists.txt` and `release_notes.json`

JSON to parse:
``` [
  {
    "full_version": "",
    "release_notes": {
      "is_critical": true,
      "application_version": 2,
      "major_revision": 0,
      "minor_revision": 0,
      "build_number": 201917060,
      "settings_version": 0,
      "download_link": ""
    }
  },
  {
    "ticket": "PBWIN-100",
    "release_notes": {
      "whats_new": [
        "Save erased file paths to encrypted database instead of text file"
      ]
    }
  },
  {
    "ticket": "PBWIN-232",
    "release_notes": {
      "whats_new": [
        "Use all CPU cores for files shredder"
      ]
    }
  }
]
```

1. Let's store JSON in a variable `${RELEASE_NOTES}` in CMake script. It can be parsed with following lines of code.
``` cmake
include(JSONParser.cmake)
```

2. Name of variable not string itself is passed to macro sbeParseJson

```
file(READ release_notes.json RELEASE_NOTES)
sbeParseJson(release_notes RELEASE_NOTES)
```

3. Now you can use parsed variables.

```
foreach(var ${example})
    message("${var} = ${${var}}")
endforeach()
```

4. When you want to access concrete JSON object, use the names in JSON, the array indexes for JSON object MUST be given after '_'
e.g if you want to store value of object 'id' of first element of array 'items' in object 'menu'
```
set(MY_VERSION ${release_notes_0.release_notes.application_version})
```

5. %.g if you want to store 1-th element of array 'short' in object 'label' of 2-th element of array items in object menu 
```
set(ShortLabel ${example.menu.items_2.label.short_0})
```

6. When you are done, clean parsed variables
```
sbeClearJson(example)
```

7. Macro `sbeParseJson` creates following variables and its values (`variable name` = `value`).
```
release_notes_0.release_notes.application_version = 1
release_notes_0.release_notes.major_revision = 0
release_notes_0.release_notes.minor_revision = 8
release_notes_0.release_notes.settings_version = 0

```

