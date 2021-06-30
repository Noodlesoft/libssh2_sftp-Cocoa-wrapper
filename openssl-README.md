# OpenSSL for Hazel

OpenSSL is a git submodule checked out in the `openssl` directory. OpenSSL is a git submodule checked out in the `openssl` directory. It's a fork of the main
OpenSSL repo. The older fork, based on Karelia's fork of OpenSSL, has been deleted.

As of June 2021 the currently merged tag is `OpenSSL_1_1_1k`. See `openssl/README` for the latest version info.

The only difference between Noodlesoft's OpenSSL and the core OpenSSL tag is in the `Configure` script. OpenSSL requires an absolute path for the `--prefix` argument while Hazel need to use `--prefix="@loader_path/../Frameworks"` in order to embed OpenSSL in a framework that's part of an app. Removing the absolute path check breaks some OpenSSL Makefile targets, but Hazel doesn't use those targets. See https://github.com/openssl/openssl/issues/15856 for some discussion of this.

Build OpenSSL using `OpenSSL.xcodeproj` using the `openssl` target. No special steps are needed. Xcode build scripts handle building OpenSSL, and the build products are `libcrypto.dylib` and `libssl.dylib`, copied to this directory. It's a lipo'd fat binsry with x86_64 and arm64 code. The `openssl` target depends on other targets that clean and then build for both architectures.

To update to a newer version of OpenSSL:

    cd openssl
    git remote add upstream https://github.com/openssl/openssl.git
    git fetch upstream

Then merge the desired commit or tag into the `noodlesoft` branch using the Git tool of your choice.


## Older notes from Karelia builds:

How to build openssl for Sandvox:
1. Update the source to a new version/tag if desired.
2. Select the target "openssl" in the Scheme popup.
3. Build. Just a regular Cmd-B build. No archiving or anything.
    Because the builds are controlled by scripts, not Xcode, they always build for "release" with -O3, but also with debug info (such as it is with -O3).
    Note also that the build outputs are:
        libcrypto.dylib & libcrypto.dylib.dSYM
        libssl.dylib & libssl.dylib.dSYM
        openssl-build-include (the headers applicable to using this exact build of openssl)
    All we're interested in is the libraries, not the app, etc.
4. Make a new git commit in SFTP if needed.

After updating/rebuilding OpenSSL, you should update/rebuild any libraries that depend on it, such as libssh2.



As of June 24, 2013, we've updated to the git submodule of OpenSSL (from the older CVS-based tarballs).
THE FOLLOWING DIRECTIONS ONLY APPLY TO OLDER, CVS TARBALL BUILDS.

OpenSSL is not a git submodule because openssl.org is a little behind the times.
They still use CVS.

So, the way to update the version of openssl we compile is:
1. Download a new version of the source distribution and corresponding MD5 file. 
    http://www.openssl.org/source/
2. Verify the MD5.
3. Unpack the tgz. You should get a folder named something like "openssl-1.0.1c".
4. Replace the entire contents of the "openssl" folder in this folder with the contents of the unpacked distribution.
5. Make a new git commit, etc.

You can check the version of the OpenSSL source by looking in the README file.

