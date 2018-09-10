# lunarg-packaging-scripts
Misc scripts used in building specific vulkan packages for LunarG.  Call scripts with `-h` or `-?` args for usage help.

- <b>`.pbuilderrc`</b>

    A symlink to this should go in your home directory, to make the `cowbuild-\*` scripts work properly

- <b>`.aptly.conf-template`</b>

    This file should be copied, modified, and placed in `~/.aptly.conf`, to make the `aptly-\*` scripts work properly

- <b>`.common-lunarg.sh`</b>

    Common variables used by all the following scripts.

- <b>`cowbuild-package-lunarg`</b>

    <b><i>Usage:</i></b> &nbsp; `cowbuild-package-lunarg [-b <build version>]`

    This script takes one optional argument of a build version number that will be appended to the end of the package version.  It makes the following hard-coded assumptions:

    - It must be executed inside the target package source directory.
    - It expects the source to be in a git working directory, and be in a clean state.
    - The debian/changelog file must have an `UNRELEASED` entry on top, with a clean numeric package version string (i.e. `1.2.3-1`, not `1.2.3-1xenial2`), and must be checked into `HEAD` in the git repository (the script calls `git reset --hard` before modifying the changelog and building).
    - it assumes there exists an updated `$DISTRO-amd64` pbuilder chroot for each distro named in `.common-lunarg.sh`
    - It assumes the default pbuilder/cowbuilder chroot location: i.e. `/var/cache/pbuilder`
    - It depends on the `~.pbuilderrc` config file in this repository existing (can be a symlink) in the user's home directory.

- <b>`cowbuild-update-chroots`</b>

    Simple script to update the chroots mentioned above.  This should be run before the first build of the day, or any time the dependent repositories are updated.

- <b>`aptly-add-pbuilder-lunarg`</b>

    This script takes no arguments.  It simply looks for any `*.changes` files under the pbuilder directory (`/var/cache/pbuilder`), and adds them to the appropriate aptly base repos

- <b>`aptly-snapshot-lunarg`</b>

    <b><i>Usage:</i></b> &nbsp; `aptly-snapshot-lunarg <prefix>`

    This script takes a uniquely named immutable snapshot of all the appropriate aptly repos.  The snapshot name must be the only argument to this script, and must be uniquely named.

- <b>`aptly-publish-lunarg`</b>

    <b><i>Usage:</i></b> &nbsp; `aptly-publish-lunarg [-p <prefix>] [-s <snapshot_prefix>]`

    This script publishes either a snapshot, or the base repo into the "rootDir" defined in `~/.aptly.conf`.  By default, publishes base LunarG repositories to `${rootDir}/testing`.  The publish prefix can be overwritten with the "-p" option, and publishing a snapshot may be accomplished with the "-s" option.
