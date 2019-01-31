# json-voorhees

This is the Stateless fork of json-voorhees. Instead of maintaining a normal git
fork we use [stgit](http://www.procode.org/stgit/) to maintain a patchset. This
simplifies things by keeping the upstream history entirely outside of our repo
and the set of stateless-specific patches is very clear. This should make it
easy to incorporate our changes upstream while pulling in new changes from
upstream regularly.

# Obtaining the Patched Code

First, install `stgit` through your package manager, e.g. `apt-get install
stgit` or `zypper in stgit`. Alternatively, you can clone the
[stgit repo](https://github.com/ctmarinas/stgit.git) and install it manually if
you want to run the latest and greatest version.

Next, run the following commands.

```
git clone git@github.com:BeStateless/json-voorhees.git
cd json-voorhees
./patch
```

Now the patched code is in the `json-voorhees` directory in the repository root
with stgit set up on the tip of the patch set.

# Making New Changes

To make a new patch simply use `stg new patch-name.patch`, edit the files you
want to include in the patch, use `stg refresh` to incorporate those changes
into the patch, and finally use `stg export -d ../patches` from the inner
`json-voorhees` directory to export the new patch. Modifying an existing patch,
reordering patches, rebasing onto newer upstream changes, and other operations
are explained in the
[stgit tutorial](http://procode.org/stgit/doc/tutorial.html).

# Making a Release

Obtain the patched code and change to the code directory.

First, run the unit tests and make sure they pass:

```
./config/dev-env --distro debian-9 -- ./config/run-tests
```

If the tests pass, then create Debian packages for json-voorhees

```
./config/make-package --dockerize debian-9
```

This will produce the following files:

* build-release-debian-9/debian-9-libjsonv1.3.deb
* build-release-debian-9/debian-9-libjsonv1.3-dev.deb

Finally, create a release using GitHub's web UI and upload those Debian
packages.

Once the release has been created, the Dockerfile in stateless-nf can be updated
to install the newer version of json-voorhees.
