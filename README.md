> This project has two homes.
> It is ok to work in github, still, for a better decentralized web
> please consider contributing (issues, PR, etc...) throught:
>
> https://gitlab.esy.fun/yogsototh/stackage

---


stackage
========

"Stable Hackage," tools for creating a vetted set of packages from Hackage.

__NOTE__ This repository is for package authors to get their code into
Stackage. If you simply want to use Stackage as an end user, please follow the
instructions on http://www.stackage.org/.

A note about the codebase: the goal is to minimize dependencies and have
the maximum range of supported compiler versions. Therefore, we avoid
anything "complicated." For example, instead of using the text package,
we use Strings everywhere.

Get your package included
-------------------------

In order to get your package included in the set of stable packages, you should
send a pull request against this repository. In the `Stackage.Config` module,
there's a function called `defaultStablePackages`. In general, to add a set of
packages, you would add:

    mapM_ (add "your-email-address") $ words
        "package1 package2 package3"

You can follow the examples of the other sets of packages in that function.
Once you've done this, you can confirm that your newly added packages are
compatible with the rest of stackage by building the package set following the
instructions below.

__NOTE__: In order to ease the process of adding new packages, we no longer
require new submissions to be tested on your own system before sending a pull
request. If you believe your package works with the newest versions of all
dependencies, you may send a pull request without testing first. If you do so,
please be sure to state this in the pull request so that the Stackage
maintainers (e.g., Michael) will know to do basic sanity checking before
merging.

You should also read the [maintainers
agreement](https://github.com/fpco/stackage/wiki/Maintainers-Agreement).

Build the package set
---------------------

As this project is just starting, we don't really have a solid set of steps. In
general, the following set of commands should be good for getting started:

    cabal update
    git clone https://github.com/fpco/stackage
    cd stackage
    cabal sandbox init # requires cabal-install 1.18
    cabal install --only-dependencies
    cabal configure
    cabal build
    ./patching/scripts/create-tarballs.sh
    ./dist/build/stackage/stackage select
    ./dist/build/stackage/stackage check
    ./dist/build/stackage/stackage build # takes a *long* time
    ./dist/build/stackage/stackage test # also takes a *long* time

Notes
-----

Make sure to have Cabal-1.16 installed in either your global or user database,
regardless of any sandboxing, as custom build types require it to be present.
You must build with cabal-install 1.16, due to several important bug fixes.

Using a non-Haskell Platform versions of GHC
--------------------------------------------

By default, Stackage bases itself off of the Haskell Platform for determining
which packages are core packages, and locks down package versions to match the
Haskell Platform selections. This works fine when you are compiling with the
same version of GHC as the Haskell Platform was built on. If you're using a
different version of GHC, you'll probably need to use the following options for
the `select` call:

    --no-platform --use-global-db

The former says to disregard Haskell Platform package versions, and the latter
says to determine which packages are core packages based on their presence in
the global package database.
