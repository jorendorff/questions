## Package Management

### Nix

[Nix: A Safe and Policy-Free System for Software Deployment](https://edolstra.github.io/pubs/nspfssd-lisa2004-final.pdf).

*   **Does the hash cover the Nix-expression used to compute the
    derivation, or just the derivation (the output of the expression)?**

    Just the derivation, surely.

*   **Other package managers allow patching a dependency. If something
    is patched in Nix, everything downstream has to be rebuilt, right?**

    Yes.

*   **What prevents a single executable from being linked against
    multiple versions of the same library?**

    Nothing in Nix itself (any more than in the Linux kernel itself or
    `ld-linux.so` itself).

    In NixOS, it seems like there might be a mechanism, as follows. Any
    given revision of the `nixpkgs` repository will contain a single
    explicit source hash, a SHA-256 or something, for each
    package. There's no obvious way for any Nix-expression in the whole
    repository to end up accidentally referring to any other
    version. (Maybe if it was in your build environment to begin?)  In
    particular, the use of `callPackages` makes it seem like
    dependencies are populated from a single directory.

    When you upgrade something, this basically ensures you are
    rebuilding everything downstream.

    It also suggests you can't just update one application, even if it's
    a leaf application, because there's no such thing. Everything is
    depended upon by your environment. You will always be on some
    specific revision of `nixpkgs`, so if you want an environment that
    specifies its ingredients from that repository, it seems impossible
    to avoid getting the exact required version of everything.

    If you built an environment using `.nix` files taken from many
    different revisions of `nixpkgs`, that would reintroduce the issue
    of multiple versions being accidentally present, indirectly, in the
    environment, and therefore possibly in the same process.

    This could also happen if you install a package from a URL.

*   **Where can I look at a Nix expression for something really
    complicated, like Firefox?**

    In the NixOS/nixpkgs repository.

    [Subversion](https://github.com/NixOS/nixpkgs/blob/master/pkgs/applications/version-management/subversion/default.nix).

    [Firefox](https://github.com/NixOS/nixpkgs/blob/master/pkgs/applications/networking/browsers/firefox/common.nix).

*   **What about "retained" dependencies upon executables like `/bin/sh`?**

    Right, scanning won't find these. The paper does mention that this
    is a weakness, but I don't quite understand what it then says about it.

    I kind of wonder if they use a hacked libc where `system` and
    `popen` embed the Nix path to a Nix-built `sh` executable.
    Presumably that would be a cyclic dependency...

*   **What about run-time dependencies other than `.so` files? Python
    modules, anyone? The paper doesn't seem to acknowledge how tied in
    this system is to knowledge about how the system works.**

*   **What about run-time dependencies on specific `.so` files that are
    dynamically loaded?**

*   **What about build-time dependencies on headers? The paper is vague
    about the distinction between build-time and run-time
    dependencies.**

*   **How is the set of derivate files specified? That is, how does Nix
    know which build files are the output?**

*   **What other information is considered part of the derivate, other
    than filenames and the bytes in those files? Empty directories? Mode
    bits? Setuid bit? Owner and group? Hard links and symlinks? A more
    concrete way of phrasing this question is: what information exactly
    do `push` and `pull` preserve?**

*   > A Nix *user environment* consists of a selection of applications
    > from the store currently relevant to a user. [...] This selection
    > may be implemented in various ways, depending on the interface used
    > by the user.

    **The only example "interface" offered is "the `PATH`
    interface". What other interfaces are there? What is this saying?**

*   **Are the store expressions always stored in the store? Not a joke
    question.**

*   **In NixOS, who can write to the store? (It's a policy question, so
    I wouldn't expect Nix itself to address it.)**

*   **How exactly are specific versions of e.g. OpenSSL injected into
    the Nix-expressions for software that depends on it? This is really
    a question of how composition is engineered in particular systems I
    guess. How does NixOS do it?**

*   **What other projects (besides NixOS) use Nix?**

*   **How are environments expressed? A build script that does a lot of
    `ln -s`?**

    In the paper, it's a Perl script. So basically, yes.

*   **What does `nix-env --install` do? The paper makes the process of
    putting files in the store quite clear, but this command—does it
    modify an existing user environment in-place? How does it know what
    to do with the derivate files? Is installation a commutative
    operation?**

*   **How are separate user and build environments expressed?**

    I think the thing they hoped to have gotten right in the design of
    Nix, up to 2004, was to support multiple environments, parallel
    installs, and atomic environment upgrade cleanly *at all*.

    You can build an environment as a derivate. And "using" an
    environment is really just a matter of setting some environment
    variables (though you can use `nix-env` if you want). So it's
    crude—you can't say "make me an environment from base environment
    `7a54bf3` plus these six packages" as you might like—but

*   **But that answer doesn't work. At every step of building a system
    with complex dependencies, we need a build environment.**

    **Suppose we are using Nix for source distribution (no cached
    builds). For each build, previously installed packages have to
    appear in the environment we use to build the next package. How do
    we express these environments?**

    Maybe every Nix expression has to describe how to build against the
    specific previously-built headers, libraries, and toolchains, so
    that we actually don't create an environment like a user
    environment, but rather use fully explicit and versioned paths for
    everything.

*   **On Linux there are supposed rules about ABI compatibility of `.so`
    libraries. Does Nix start from the assumption that these rules don't
    work?**

*   **This bit about "atomicity"... this seems like an improvement, but
    atomicity still isn't the guarantee you want, right? Running with a
    symlink on your `PATH` seems to leave the possibility that during a
    single run of a program, things will change out from under you.**

*   **I assumed we were very soon going to be talking about chroot jails
    and containers. No? Did this come along later? Or never?**

*   **How does a build-support library written in the Nix language,
    like `pkgs/build-support/fetchsvn`, express its dependency on
    Subversion?**

    At a guess I bet you have to pass in a `subversion` derivation to
    get it to run.

*   **An improvement like changing from a single output path to multiple
    paths is a "schema change", so it changes all hashes, and requires
    everyone to rebuild or re-download everything, right?**

    nbp: Correct.

*   **[This blog post](https://www.tweag.io/blog/2020-09-10-nix-cas/)
    describes modifying Nix to use content-addressing for built files.
    But how exactly will that work? Nix scans binaries for filenames,
    and currently the filenames contain derivation hashes. Will those
    change to be content hashes?**


#### The Nix-expression language

*   **Hey, this language looks pretty nice! Does it have any
    questionable bits?**

    Sure, it's a programming language, of course it's got its
    questionable bits.

    None of this stuff impacts the invariants of the underlying store,
    though, because what's hashed is the *output* of the Nix-expression,
    so that the language can evolve without invalidating your store.

    -   It has a `with` expression which introduces dynamic scoping,
        like JavaScript's `with` statement. It's terribly, terribly
        handy for uses like

        ```nix
        # Runtime dependencies.
        LD_LIBRARY_PATH = with pkgs.xlibs; stdenv.lib.makeLibraryPath [
          libX11 libXcursor libXi libXrandr vulkan-loader
        ];
        ```

        Still, dynamic scoping is bad. Tacitly acknowledging this, the
        language has some mitigations for the badness
        (e.g. `with`-bindings don't shadow `let`-bindings), which are
        themselves questionable. It would have been better, I think, to
        require a special sigil when you use a with-binding, like
        `@libX11`. VB.Net's `With` statement works this way.

    -   `rec` works about the same way, but it's not that bad. The
        scoping is static unless you mix in computed keys (which the
        language also has).

    -   Paths that start with `~/` are expanded using the user's home
        directory. (I kind of wonder how they're getting that. `$HOME`?
        `getpwuid(3)`?) Iffy because context-dependent.

    -   Paths enclosed in `<angle/brackets>` are resolved by searching
        along your `NIX_PATH`.

    -   Lexically, paths don't have to be quoted. So I guess `a/b.sh` is
        a path but `a / b.sh` is division? I honestly don't mind stuff
        like this. It makes sense visually.

    Apart from that it's just really nice.

*   **How do path values differ from strings? Why are they different?**

*   **What comes out when you antiquote a derivation? Do you get the Nix
    store path?**

*   **Does it matter that the language is lazy? Doesn't everything tend
    to be used eventually, and aren't eager languages easier to debug?**

    Some stuff in `.nix` files is like `fetchurl` which might be
    fetching lots of megabytes, so yeah it probably matters there.

    Laziness probably makes the recursive features `let` and `rec`
    nicer.
