include(JSONParser.cmake)  

# Read and parse release_notes.json
file(READ release_notes.json RELEASE_NOTES)
sbeParseJson(release_notes RELEASE_NOTES)

# Get application and settings versions from release_notes.json
set(MY_VERSION ${release_notes_0.release_notes.application_version})
set(MY_MAJOR_REVISION ${release_notes_0.release_notes.major_revision})
set(MY_MINOR_REVISION ${release_notes_0.release_notes.minor_revision})
set(SETTINGS_VERSION ${release_notes_0.release_notes.settings_version})

message("Application version: " ${MY_VERSION})
message("Major revision: " ${MY_MAJOR_REVISION})
message("Minor revision: " ${MY_MINOR_REVISION})
message("Settings version: " ${SETTINGS_VERSION})

