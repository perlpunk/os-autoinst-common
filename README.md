# Common files for os-autoinst/os-autoinst and os-autoinst/openQA

This repository is to be used as a
[git-subrepo](https://github.com/ingydotnet/git-subrepo).


`git-subrepo` is available in the following repositories:

[![Packaging status](https://repology.org/badge/vertical-allrepos/git-subrepo.svg)](https://repology.org/project/git-subrepo/versions)

and in `devel:openQA`, e.g. https://download.opensuse.org/repositories/devel:/openQA:/Leap:/15.2/openSUSE_Leap_15.2/

## Usage

### Clone

To use it in your repository, you would usually do something like this:

    % cd your-repo
    % git subrepo clone git@github.com:os-autoinst/os-autoinst-common.git ext/os-autoinst-common

This will automatically create a commit with information on what command
was used.

And then, if necessary, link files via symlinks to the places where you need
them.

The cloned repository files will be part of your actual repository, so anyone
cloning this repo will have the files automatically without needing to use
`git-subrepo` themselves.

`ext` is just a convention, you can clone it into any directory.

It's also possible to clone a branch (or a specific tag or sha):

    % git subrepo clone git@github.com:os-autoinst/os-autoinst-common.git \
        -b branchname ext/os-autoinst-common

After cloning, you should see a file `ext/os-autoinst-common/.gitrepo` with
information about the cloned commit.

### Pull

To get the latest changes, you can pull:

    % git subrepo pull ext/os-autoinst-common

If that doesn't work for whatever reason, you can also simply reclone it like
that:

    % git subrepo clone --force git@github.com:os-autoinst/os-autoinst-common.git ext/os-autoinst-common

### Making changes

If you make changes in the subrepo inside of your top repo, you can simply commit
them and then do:

    % git subrepo push ext/os-autoinst-common

### Possible Workflows

One can make changes to the subrepo directly from the host repo, but there are
different strategies.

#### Make changes in host repo first and push after approval

* Create a branch in the host repo
* Make changes in the subrepo directory
* Create PR
* After approval, merge and push changes to subrepo


Prepare:

    % git checkout -b feature1
    # Pull subrepo changes first to avoid merges later
    % git subrepo pull external/os-autoinst-common
    # This will create a commit automatically if there are any changes
    # If there are functional changes, it might be good to do this in an extra
    # PR first

Make changes:

    % touch external/os-autoinst-common/foo
    % git add .
    % git commit
    % git push -u fork feature1
    # Create PR

When the PR is merged, push the changes directly to the subrepo master if you
have the allowance.

Check if there were any changes in the subrepo in the meantime with

    % git subrepo pull external/os-autoinst-common

Push:

    % git subrepo push external/os-autoinst-common

#### Make changes in a subrepo fork/branch

This will allow to test the PR in several host repos.

First create a branch in your `os-autoinst-common` fork (or in `origin`, if ok).

    % cd /path/to/os-autoinst-common
    % git pull
    % git checkout -b feature1
    % git push -u fork feature1

Make changes in the host repo, e.g. `openQA`:

    % cd /path/to/openQA
    % git checkout -b feature1
    % git subrepo clone -f git@url-to-your-fork -b feature1
    % touch external/os-autoinst-common/foo
    % git add .
    % git commit
    % git subrepo push external/os-autoinst-common
    % git push -u fork feature1
    # Create WIP openQA PR to test the changes
    # Create os-autoinst-common PR

When the `os-autoinst-common` PR is merged, just reset your `openQA` `feature1`
branch to master and do

    % git subrepo pull external/os-autoinst-common

## git-subrepo

You can find more information here:
* [Repository and usage](https://github.com/ingydotnet/git-subrepo)
* [A good comparison beween subrepo, submodule and
  subtree](https://github.com/ingydotnet/git-subrepo/blob/master/Intro.pod)


## License

This project is licensed under the MIT license, see LICENSE file for details.
