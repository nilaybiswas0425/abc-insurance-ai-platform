# Day 1 -- Git and GitHub Setup

**Date:** November 24, 2026\
**Project:** ABC Insurance -- Enterprise Claims Intelligence Platform\
**Repository:** `abc-insurance-ai-platform`

## Completed

-   Created the local project directory.
-   Initialized Git and added `README.md`.
-   Created the first commit: `06e5c54`.
-   Created the GitHub repository and configured `origin`.
-   Renamed the local branch to `main`.
-   Installed GitHub CLI (`gh`) and authenticated through the browser.
-   Verified that the remote push succeeded and the repository is
    available on GitHub.

## Issues encountered and resolutions

  -------------------------------------------------------------------------------------
  Issue                               Cause                   Resolution
  ----------------------------------- ----------------------- -------------------------
  `Repository not found`              The remote URL had been Set `origin` to the plain
                                      entered with Markdown   HTTPS repository URL.
                                      link formatting instead 
                                      of a plain URL.         

  `403 Permission denied`             Git operations were not Installed GitHub CLI and
                                      yet authenticated       completed browser
                                      successfully with       authentication.
                                      GitHub.                 

  `gh: command not found`             GitHub CLI was not      Installed it using
                                      installed.              Homebrew:
                                                              `brew install gh`.

  Homebrew appeared paused at         Homebrew was building   Allowed the installation
  `./make.bash`                       the Go dependency from  to continue.
                                      source.                 

  `~/.config/gh: permission denied`   GitHub CLI could not    Resolved the local
                                      create its              configuration-directory
                                      configuration directory permission issue and
                                      because of local        completed setup.
                                      directory permissions.  
  -------------------------------------------------------------------------------------

## Key commands

``` bash
# Initialize the local repository
git init

# Stage and commit the README
git add README.md
git commit -m "Day 1: Initialize ABC Insurance AI Platform"

# Configure the GitHub remote
git remote add origin https://github.com/nilaybiswas0425/abc-insurance-ai-platform.git

# Rename the branch and push
git branch -M main
git push -u origin main

# Verify the remote and GitHub CLI authentication
git remote -v
gh auth status
```

If `origin` is already configured, update it rather than adding it
again:

``` bash
git remote set-url origin https://github.com/nilaybiswas0425/abc-insurance-ai-platform.git
```

## Final status

-   Local Git repository: **Complete**
-   Initial commit: **Complete**
-   GitHub authentication: **Complete**
-   Remote push: **Successful and verified**
-   GitHub repository:
    `https://github.com/nilaybiswas0425/abc-insurance-ai-platform`

**Day 1 Git/GitHub setup is complete.**
