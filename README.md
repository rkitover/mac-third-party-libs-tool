# NAME

third_party_libs_tool - bundle and link third party dylibs into a Mac OS X app bundle

# SYNOPSIS

third_party_libs_tool [--list] ./MyApp.app

# DESCRIPTION

This will help you bundle up e.g. a compiled binary that uses
Homebrew/MacPorts/Fink dynamic libraries.

This tool will recursively scan binaries and dylibs under
`MyApp.app/Contents/MacOS`, find any dependent third party dylibs, and
recursively scan them for other dylibs, copy the found dylibs into
`MyApp.app/Contents/Frameworks`, and fix up all dynamic linkage stuff for both
the binaries and the copied dylibs.

Linking to multiple versions of a library is now supported.

The searching etc. can be made more flexible, PRs welcome.

With `--list` only scan for dylibs, do not copy or link anything.

Version 0.8 is much faster than previous ones.

**WARNING:** linking may require sudo, but most likely will not.

**WARNING:** it is your responsibility to ensure that the licenses for the
libraries that end up bundled in your `.app` permit redistribution.

# USING WITH CMAKE

You can easily automate invoking this script from cmake in a `POST_BUILD` step
using something like this:

```cmake
SET(MYAPP_ICON myapp.icns)
SET(MYAPP_ICON_PATH ${CMAKE_CURRENT_SOURCE_DIR}/icons/${MYAPP_ICON})

ADD_EXECUTABLE(
    myapp
    MACOSX_BUNDLE
    $(MYAPP_SOURCES}
    $(MYAPP_RESOURCES}
    ${MYAPP_ICON_PATH}
)

TARGET_LINK_LIBRARIES(
    myapp
    ${MYAPP_LIBRARIES}
)

IF(APPLE)
    SET(MACOSX_BUNDLE_ICON_FILE ${MYAPP_ICON})
    SET_SOURCE_FILES_PROPERTIES(${MYAPP_ICON_PATH} PROPERTIES MACOSX_PACKAGE_LOCATION Resources)

    ADD_CUSTOM_COMMAND(TARGET myapp POST_BUILD
        COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/tools/osx/third_party_libs_tool "$<TARGET_FILE_DIR:myapp>/../..")
ENDIF(APPLE)
```

# LICENSE

BSD 2-Clause

# CONTRIBUTING

This program is written in POSIX sh, please do not submit PRs with bashisms. See
the standard for details. If you have doubts, run it through `dash`.
