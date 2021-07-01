# libssh2 for Hazel

Libssh2 is a git submodule checked out in the `libssh2` directory. Its source is the code libssh2 project at https://github.com/libssh2/libssh2. Hazel does not make any changes to libssh2. Previously, it was a fork of the Karelia `libssh2`, but it does not depend on or relate to Karelia's repository any more.

The library has been updated to add ed25519 key support, which was added as of libssh2 1.9.0. See `libssh2/RELEASE-NOTES` for the latest version info.

As of June 2021 the submodule is pinned to a more recent commit than the 1.9.0 tag. This is because of a bug in the configure script in 1.9.0 that was resolved after the tag in May 2021. See https://github.com/libssh2/libssh2/pull/594 for discussion. It would be good to update Hazel's libssh2 to a newer tag, when a newer tag is created. The submodule is using `origin/master` commit `a88a727.

Build libssh2 using `libssh2.xcodeproj` using the `libssh2` target. No special steps are needed. Xcode uses build scripts to compile and link the library. The build product is `libssh2.dylib`, which is copied to this directory. It's a lipo'd fat binsry with x86_64 and arm64 code. The `libssh2` target depends on other targets that clean and then build for both architectures.

## Older notes from Karelia builds:

libssh2 is a git submodule - be sure to update it via git

How to build libssh2:
1. Update the source to a new version if desired.
2. Select the target "libssh2" in the Scheme popup (32- or 64-bit doesn't matter).
3. Build. Just a regular Cmd-B build. No archiving or anything.
    Because the builds are controlled by scripts, not Xcode, they always build for "release", but also with debug info.
    Note also that the build outputs are:
    	libssh2.dylib  &  libssh2.dylib.dSYM
4. Make a new git commit if needed. Git is tracking the built dylib & dSYM as well as the source.

After updating/rebuilding libssh2, you should update/rebuild any libraries, frameworks, or apps that depend on it, such as libcurl & SFTP.
