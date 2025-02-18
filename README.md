## JSON parser for CMake

Module JSONParser.cmake contains macro `sbeParseJson` to parse JSON string stored in given variable (second argument). 
Macro fills given variable (first argument) with list of all names in JSON string. For each name it creates variable and sets the value from JSON string.


> **JSON file content must be read manually.**

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

1. Include our parser
``` cmake
include(JSONParser.cmake)
```

2. Let's store JSON in a variable `${RELEASE_NOTES}` in CMake script. It can be parsed with following lines of code.
```
file(READ release_notes.json RELEASE_NOTES)
sbeParseJson(release_notes RELEASE_NOTES)
```

3. Now you can enumerate parsed variables.

```
foreach(var ${release_notes})
    message("${var} = ${${var}}")
endforeach()
```

4. When you want to access concrete JSON object, use the names in JSON, the array indexes for JSON object MUST be given after '_'
```
set(MY_VERSION ${release_notes_0.release_notes.application_version})
```

5. In the other words, if you want to address 0-th element of array 'whats_new' in the object 'release_notes' of 1-th element of array items of the parsed file 
```
release_notes_1.release_notes.whats_new_0 = Save erased file paths to encrypted database instead of text file
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

