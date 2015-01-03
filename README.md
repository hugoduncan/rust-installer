[![Build Status](https://travis-ci.org/rust-lang/rust-installer.svg?branch=master)](https://travis-ci.org/rust-lang/rust-installer)

A generator for the install.sh script commonly used to install Rust in
Unix environments. It is used By Rust, Cargo, and is intended to be
used by a future combined installer of Rust + Cargo.

# Usage

```
./gen-installer.sh --product-name=Rust \
                   --verify-bin=rustc \
                   --rel-manifest-dir=rustlib \
                   --success-message=Rust-is-ready-to-roll. \
                   --image-dir=./install-image \
                   --work-dir=./temp \
                   --output-dir=./dist \
                   --non-installed-prefixes=foo,bin/bar,lib/baz \
                   --package-name=rustc-nightly-i686-apple-darwin \
                   --component-name=rustc \
                   --legacy-manifest-dirs=rustlib \
                   --bulk-dirs=share/doc
```

Or, to just generate the script.

```
./gen-install-script.sh --product-name=Rust \
                        --verify-bin=rustc \
                        --rel-manifest-dir=rustlib \
                        --success-message=Rust-is-ready-to-roll. \
                        --output-script=install.sh \
                        --legacy-manifest-dirs=rustlib
```

*Note: the dashes in `success-message` are converted to spaces. The
script's argument handling is broken with spaces.*

To combine installers.

```
./combine-installers.sh --product-name=Rust \
                        --verify-bin=rustc \
                        --rel-manifest-dir=rustlib \
                        --success-message=Rust-is-ready-to-roll. \
                        --work-dir=./temp \
                        --output-dir=./dist \
                        --non-installed-overlay=./overlay \
                        --package-name=rustc-nightly-i686-apple-darwin \
                        --legacy-manifest-dirs=rustlib \
                        --input-tarballs=./rustc.tar.gz,cargo.tar.gz
```

# Future work

* Make install.sh not have to be customized, pull it's data from a
  config file.
* Install an uninstall.sh script.
* Allow components to be selected during install.
* Allow components to be modified from an existing install.
* Be more resiliant to installation failures, particularly if the disk
  is full.
* Pre-install and post-uninstall scripts.
* Make the verify-bin a per-component option.
* Be more thoughtful about overwriting existing files.
* Allow components to depend on or disallow other components.
* Sanity check that expected destination dirs (bin, lib, share exist)?
* Add --docdir flag. Is there a standard name for this?
* Remove doc directories on uninstall.
* Remove empty directories on uninstall?
* Install bulk  dirs using 'install -d', not 'cp'