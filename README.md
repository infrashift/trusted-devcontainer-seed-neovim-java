# trusted-devcontainer-seed-neovim-java

A starting point for a **remote development workspace**, as a repository: a
terminal-first Java environment -- tmux, Neovim with a pinned LazyVim --
reached over SSH. Clone it, or let the platform clone it for you, and the first
push already produces a devcontainer image the forge's CI builds and publishes
-- under your own namespace -- and a workspace that runs exactly that image.

This is a **seed**: InfraShift publishes one per language as
`github.com/infrashift/trusted-devcontainer-seed-<language>`, and the platform
mirrors each into its forge's `devcontainer-seeds` group. The vscode flavour of
this language is `trusted-devcontainer-seed-java`; this one is the Neovim
flavour.

## What is in it

    .devcontainer/          the environment: the trusted java template's features,
                            tmux + Neovim + LazyVim (lang.java), the java-tools
                            feature, plus the workspace runtime contract (see its README)
    Makefile, scripts/      the golden workflow -- the interface between this
                            repository and the forge; for a devcontainer repository
                            it validates rather than compiles
    .devcontainer/services.json
                            companion services the forge builds beside the devcontainer
                            -- a PostgreSQL (services/db/)
    .gitignore              refuses key material and SSH configuration

## The loop

1. Your workspace's project directory already holds a clone of your
   repository. `ssh -t` in and you are in tmux: Neovim on the left, a shell on
   the right.
2. Change `.devcontainer/` -- add a feature, bump a version -- or anything else.
   `make all` validates before pushing.
3. Push a branch and open a merge request. The forge's CI builds the
   devcontainer and, once merged, publishes it as
   `<your namespace>/<repository>-devcontainer:<revision>`.
4. Deploy it from the portal or the onboarding bundle
   (`./workspace-ctl.sh deploy <workspace> <revision>`). Your project directory
   survives the redeploy.

## The editor

jdtls (the Eclipse JDT Language Server, with lombok) attaches to Java buffers through nvim-jdtls; it needs a minute and ~1 GiB the first time it indexes a project.

## Package registries

Nothing to configure. `mvn` and `gradle` read the platform's Maven group through the deps PEP (`/etc/profile.d/package-proxies.sh`); see `RDW-DEV-GUIDE-JAVA.md`.
