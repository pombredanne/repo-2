Spiral repo
===========

This is the repository that stores all of Spiral Repository package configurations, using [controller](https://github.com/spiral-repo/controller).
This repository contains configurations of empty packages that provides somewhat compatibility with Debiantai sources.
Mainly for closed-source proprietary software that doesn't allow any redistribution.

Ex. Enpass doesn't allow redistribution, but it provides official Debiantai packages. Though AOSC OS uses DPKG+APT as well, AOSC OS and Debian sometimes don't share the same package name, so APT may not be able to find the correct dependency. In this case, AOSC OS doesn't have `libxss1`, which is a part of `x11-lib` in AOSC OS. This repository provides a empty package named `libxss1` but depends on the correct package, so that we can use Enpass on AOSC OS.

How to use this repository
--------------------------

1. Create a file under `/etc/apt/sources.list.d` (the suffix must be `.list`, such as `spiral.list`):
   ```
   deb https://spiral.v2bv.net stable main
   ```
2. Download the GPG public key to `/etc/apt/trusted.gpg.d/`
   ```
   # wget -O /etc/apt/trusted.gpg.d/ https://spiral.v2bv.net/spiral.gpg
   ```
3. Done!

Please do note that APT may be warning you that this repository doesn't contain any package for the architecture `amd64`, this is a expected behavior since all packages in this repository are `noarch`.

Contributing & Formatting
-------------------------

All configurations stored in [`packages`](packages) directory.
Configurations are in JSON format, for example, our first package, libxss1:

```
{
	"name": "libxss1", # Name of the package
	"deps": [
		"x11-lib" # Real dependencies in AOSC OS
	],
	"description": "included in x11-lib", # Decription
	"version": { # Way to acquire versioning information
		"method": "repology", # Acquire version from Repology
		"repology": "libxss", # Name of the package on Repology
		"distro": "debian_testing" # Follow which distro
	}
}
```

Some notes:
 - `name`, and `deps` are required fields.
 - Make sure packages listed in `deps` really exist.
 - "Empty package for Debiantai compatibility" will be appended to the beginning of the package's description.
 - If no `version` section defined, the controller will pick `9999` as the version
 - Right now there are only two methods supported for `version`:
   - `static`:
     - A field called `static` is required, just put your version there :)
   - `repology`:
     - Just like the example above.
