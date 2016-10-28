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

The searching etc. can be made more flexible, PRs welcome.

With `--list` only scan for dylibs, do not copy or link anything.

**WARNING:** linking may require sudo, but most likely will not.

**WARNING:** it is your responsibility to ensure that the licenses for the
libraries that end up bundled in your `.app` permit redistribution.

# LICENSE

BSD 2-Clause

# CONTRIBUTING

This program is written in POSIX sh, please do not submit PRs with bashisms. See
the standard for details. If you have doubts, run the script through `dash`.
