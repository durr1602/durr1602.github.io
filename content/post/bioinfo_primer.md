---
title: "Bioinfo primer"
date: 2026-08-29T15:47:26-04:00
draft: false
---
# Bioinfo primer

## Context

I have worked in academic research in biology since 2015.
I've gradually shifted to using computational approaches during my postdoc in [the lab of Pr Christian Landry](https://landrylab.ibis.ulaval.ca/) (Université Laval, Québec).
At that time, I used to mentor a lot of students who were just getting started in bioinfo.
Because they were coming from different backgrounds, they needed to learn about many concepts in a short amount of time.
To try and help them, I've written several versions of this primer over the years.
The goal has evolved over time.
This is because there is a tradeoff between learning concepts deeply and getting oriented quickly and efficiently.
To overcome this challenge, I've settled on the following compromise:
the version you're reading right now is my detailed, more personal version.
That way, labs can trim down unnecessary details and adapt based on *their* needs.
For example, some labs would rather keep using Conda, while others prefer Pixi.
Comparing the two may confuse newcomers, rather than orient them quickly.

## Can I skip all this and simply ask ChatGPT (or any other LLM) for help?

I personally don’t recommend it.
While a large language model (LLM) can be a good assistant for code development (when used correctly),
it often relies on crucial contextual clues, as well as a knowledge base that *you need to provide* to obtain a reliable answer.
You could also be tempted to simply feed this document into an LLM, which would beat the purpose of this document as a pedagogical tool to help you learn.
And learning you will if you read and practice actively.

## What this primer covers

This primer mostly covers best practices regarding:

- How to organize a computational biology project
- How to install packages
- How to manage environments
- How to start tracking files with Git

I’ve tried to frame some technical details in their respective historical context to help you understand why certain things are the way they are now, which may even give you insight into how they are likely to evolve.

## What this primer does not cover

This primer does not cover much any aspect related to the use of Graphical User Interfaces (GUIs).
Some of the things discussed here can be done using GUIs.
You may decide the modalities you prefer once you know what lies beneath.
In computational biology, we are lucky to have tools at our disposal to help us conceive things programmatically.
For example, instead of adjusting the esthetics of a plot by hand, you can do the same thing with code.
This is not only much quicker on the long run, but also contributes to **reproducibility**, which is foundational to scientific research.
The same can be said of data processing.
While you may find Excel practical for simple tasks, it does not keep track of any modification brought to the data,
which makes it very easy to introduce errors.
In general, GUIs simplify some processes, in exchange for tunability.

This primer does not cover the use of external tools/CLIs (e.g. bowtie), with varying installation and usage requirements.

Finally, this primer does not cover the use of high-performance computing (HPC) clusters (for now).

## Where to start?

I’m assuming you’re about to embark on a bioinformatic journey.
This means you’ll either lead or collaborate with others on a computational biology project.
A good way to start is to read [this article](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1000424) which is surprisingly still relevant at the time of writing.
Below is a brief summary:

### How to organize computational biology projects

Basic principles:
- How files are **organized** and **named** informs a lot on *what* has been done, and *why*
- Most of what you do, you'll have to do over again
- Nothing should be duplicated, to ensure proper tracking of changes over time

Recommended way to organize files and directories:

1. **LEVEL 1**: ONE PROJECT = ONE ROOT DIRECTORY = ONE GIT REPOSITORY
1. **LEVEL 2**: one type of file = one subdirectory
   - `data/`: **fixed** datasets, small size only. A `README.md` file explains:
     - how the data was retrieved (URL/source, version, date)
     - where large datasets or databases are located (mounted filesystem, archive, etc)
   - `results/`: analyses output
   - `docs/`: any documentation
   - `scripts/`: scripts that are specific to this project
   - `config/`: optional but very useful, especially for pipelines,
     can contain a sample layout, a `config.yaml` with pipeline parameters, etc

I'm personally not a fan of putting either dates or numbers in filenames.
The whole point of version control (see corresponding section below) is to make sure we can track versions across time.
Commits should be the source of truth for *when* a file was produced, especially because it's tightly linked to *how* it was produced.
Some recommend to number scripts based on their order of execution. But this order may change as the project evolves.
I would argue it's not straightforward to keep a full history of versions across one or several renaming events.
There are different ways to communicate a workflow without having to rely on filenames (e.g. documenting the flow diagram).

### Resources to learn programming

Next, we should talk more specifically about what shall occupy most of your time as an aspiring bioinformatician: **data analysis** and **visualization** using programming.
This primer specifically focuses on the two most popular programming languages in bioinfo (at the time of writing): Python and R.

There are many resources out there to learn about Python and R.
However, even the best coder in any of these languages needs to know the basics in Bash,
since most tasks that don’t relate directly to coding lie in a Unix shell.

If you need a refresher on any of these, check out the core lessons on [Software Carpentry](https://software-carpentry.org/lessons/).

[This repo](https://github.com/ebookfoundation/free-programming-books) also contains a multitude of free books on programming.

## R🤝Python

Much like many chemists back in the days would be considered biologists by today’s standards,
many scientists were pioneering the field of bioinformatics [before the term was even coined](https://www.asimov.press/p/bioinformatics).
At that time, low-level compiled languages like C or Fortran were better suited to exploit the limited capabilities of available hardware.
With biological datasets growing in size and complexity, the burgeoning field shifted toward higher-level environments that prioritized rapid data exploration and readability.
While Perl was the most popular language in bioinformatics in the late 1990s, the turn of the century introduced new technologies that required new ways to manipulate the massive data being produced.
Importantly, there was now a need to manipulate massive tabular data, instead of text.
R gradually replaced Perl as the leader for statistical modeling.
Python, coming from a software engineering background, expanded a bit later into scientific computing.
Around 2011, its libraries NumPy, SciPy and Pandas became mature enough to tackle “Next-Generation Sequencing” data.

Students in bioinformatics like to engage in the friendly rivalry between the two ecosystems,
but it’s important to remember that they were built upon different foundations.
Nowadays, they largely overlap and complement each other.
R is often preferred for statistics, while Python is usually favored for scalable pipelines, machine learning / deep learning.
I  would argue that Python now has mature packages to handle both statistics and data visualization.

## A word on reproducibility

Before diving into coding, we need to consider some important points:

1. Data analysis should, like any scientific experiment, be reproducible
1. We build on the work of others so that others may in turn build upon ours 

Science is a collective effort, which means we need to make sure that our work can be shared (with our colleagues, our peers, the community…).
In a more pragmatic way, science relies on peer-reviewed publications, which means your work should at the very least be reproducible by a peer-reviewer.

The challenge is that each scientist has their own expertise and their own machine.
Therefore, we need to make sure that our code can run on different systems (or at the very least on a system with comparable requirements) and that it is documented enough that others can use it.

## System requirements

Modern laptops run on different CPU architectures, predominantly x86_64 (Intel/AMD) and ARM64 (Apple Silicon M-series, Snapdragon).
This has important implications when installing anything.
Indeed, code written in a compiled language like C, is translated by a compiler (like GCC) into architecture-specific machine code.
While code written in Python or R is interpreted, high-performance packages used in bioinformatics frequently rely on compiled underlying libraries (C/C++/Fortran).
A package binary compiled for x86_64 will not run natively on ARM64 without an emulation layer.

To manage these dependencies without system-level conflicts, we use **package and environment managers**.
System-level dependencies should be installed with the designated package managers, e.g. `homebrew` and `apt` on Mac and Debian-based Linux distributions, respectively.
I'm simplifying here (and other Linux distributions may use one of the following managers: `dnf`, `pacman`, `apk`, `zypper`, etc).
In contrast, each project should have its own workspace, which may contain one or several environments, each handling task-specific dependencies.

Language-specific tools like `uv` (Python) or `pak` (R) handle isolated language libraries and are therefore recommended for most small-scale Python or R projects.
For complex computational workflows requiring system binaries, C libraries, Python, and R in a unified environment, bioinformaticians use cross-platform package managers like `pixi` or `micromamba`, which both leverage the Conda ecosystem.
These are other cross-platform package managers out there.
But the reason why something like Nix is not mainstream in bioinfo is simply because tools have historically accumulated on different repositories.

A final note before we jump into environments: while full system containerization (e.g. using Docker or Apptainer, formerly Singularity) isolates the entire operating system layer, it’s often overkill for day-to-day work.
It’s also not incompatible with something like `pixi`, which can simply be “containerized” if production-level reproducibility becomes necessary later on.

## Environments

### Package repositories

We've mentioned that packages can be installed with `uv` or `pak`.
What we've omitted is that packages can be fetched from different sources.
To simplify, official releases can be fetched from:

- **PyPI: an uncurated index/archive** of Python packages (default source for `uv` or `pip`)
- **CRAN: a curated repository** of R packages, enforcing strict check flags (default source for `pak`)
- **BioConductor: a curated repository** of R packages, with 2 releases a year synced with R's release cycle and quality control equivalent or stricter than CRAN.

Contrary to PyPI, both CRAN and BioConductor enforce automated reverse-dependency testing (among other checks), which has historically helped minimized breaking changes.

Additionally, any version (e.g. development) of a package hosted on public hosting platforms like GitHub, can be retrieved from there directly (using `uv`, `pip`, `pak`, etc).
Whenever that's the case, there is **no quality control** enforced, even for an R package.

If you're already familiar with R, you've probably already used commands like `install.packages()` or `devtools::install_github()`.
`pak` provides a unified syntax (`pak::pkg_install()`), while also being much faster.
This is mainly because `pak` fetches pre-compiled binaries (via Posit Public Package Manager) instead of compiling from source.

Unfortunately, regardless of the language or the approach, strict or long-term reproducibility is not guaranteed.
Reinstalling any project even a few months later without fixed version constraints may pull a newer release that breaks your code.
To solve this, modern workflows use lockfiles that record the exact version, commit hash, and origin of every installed package.

- In Python: `uv` automatically generates and maintains a `uv.lock` file when resolving dependencies.
- In R: R uses `renv` to create a `renv.lock` file tracking project dependencies.
  `pak` integrates directly with `renv` (`renv::install()`).

At that point, we are almost ready to install packages and start coding!
However, for the sake of prosperity, we should discuss right away the main limitation of using only `uv` or `pak`.
While excellent for pure Python or pure R projects, they will simply not do the trick for more complex workflows mixing languages and system tools.
Indeed, `pak` relies on `apt` to install system-level libraries, and `uv` cannot natively handle CRAN dependencies. To bridge this gap, bioinformaticians have relied on the Conda ecosystem.

## Conda

Conda arose in the 2010s to solve cross-language dependency management.
Over the years, the ecosystem has spawned several variants:

- Anaconda: the original, full-featured distribution
  > [!CAUTION]
  > The company now enforces commercial licensing terms for large institutions, with academia standing on thin ice, avoid using Anaconda/Miniconda or the `defaults` channel
- Miniconda: a lightweight Anaconda distribution containing only Python and Conda.
- Miniforge: a community-led distribution using `conda-forge` as default channel, avoiding Anaconda’s terms of service
- Mamba/Micromamba: a high-performance rewrite of Conda replacing the legacy Python dependency solver with a fast C++ solver

Personally, I don't recommend any of these options.
Instead, I and many bioinformaticians have migrated to the modern, even faster Pixi (from the people behind Mamba).

### Why move from Conda to Pixi

Pixi uses a fast Rust-based solver that handles both Conda packages (including Python versions, C libraries, and R packages from `conda-forge` or `bioconda`) **and** PyPI dependencies (using `uv` as a backend).
It unifies these into an isolated workspace backed by a single deterministic lockfile (`pixi.lock`).

Key reasons to prefer Pixi over classic Conda: 

- No global shell contamination: traditional Conda requires modifying your shell configuration to inject a `(base)` environment, often causing runtime conflicts between system binaries (`curl`, `git`, `openSSL`) and Conda-installed equivalents.
  Pixi operates at the workspace level without requiring shell hook activation.
- Safe multi-language resolution: installing packages with `pip` inside a standard Conda environment frequently corrupts Conda's internal package tracking.
  Pixi natively coordinates Conda and PyPI dependencies during resolution.
- Isolated package caching: Conda relies on shared hard links from a central cache.
  If a package in the cache becomes corrupted, every environment linked to it breaks.
  Pixi isolates dependencies predictably within the project workspace.
- Lockfiles: standard Conda lacks native, deterministic lockfiles.
  Pixi generates them automatically, guaranteeing strict reproducibility
- Cross-architecture support: before relying on Rosetta 2 or x86 emulation on ARM systems (e.g., Apple M-series),
  Pixi lets you declare target platforms directly in `pixi.toml` (e.g. `platforms = ["linux-64", "osx-arm64"]`).
  If native binaries exist on `conda-forge`, Pixi resolves them automatically across architectures.

Pixi even has a built-in **task runner** that facilitates early development of pipelines.

## How to?

Detailing the exact command lines to create a new workspace and start installing packages is out of this primer’s scope.
Instead, we redirect the reader to the documentation of the main tools mentioned so far:
- [`uv`](https://docs.astral.sh/uv/)
- [`pak`](https://pak.r-lib.org/index.html)
- [`pixi`](https://pixi.prefix.dev/latest/)

## Version control

Version control is essential to any scientific endeavour.
Wet lab scientists record everything they do daily in their lab notebook (whether physical or electronic):

- The date
- The scientific question
- The proposed experiment, including detailed protocols
- The materials, including lot numbers for key reagents
- Observations, what worked and what didn’t
- Interpretations of those observations
- Proposed next steps and solutions

Dry lab scientists should follow the same discipline.
Because our work mostly lives in digital text files, we can leverage tools that automatically track every change across time.

To do this, you should use `git` (or [`jj`](https://github.com/jj-vcs/jj)) from the very first minute you start a project.
Follow these simple steps:

1. Initialize a Git repository right from the start with `git init`
1. Draft a `README.md` immediately.
   It can start as a minimal summary of even a tiny assigned task.
   Over time, it should grow to detail the *what*, the *why*, and the *how* of your project.
1. Commit every step of the way.
   1. Every time you complete as task, stage your changes with `git add`
   1. Draft a descriptive commit message with `git commit`

An important note on what to track: Git is designed for tracking plain text files (scripts, code, configuration files, and documentation).
It is not designed as a storage system for large datasets or binary files (including `.docx`, etc).
Limit your use of Git LFS as much as you can.

Do not track:
- Large data files: raw sequencing reads, large matrices, or model weights
- External databases: do not commit local copies of reference databases.
  Instead, document the exact version, URL/source and retrieval date in your documentation (usually `data/README.md`)
- Temporary outputs
- Environment directories (e.g. `.venv/`, `.pixi/`)

You don’t need to manually filter out unwanted files every time you stage changes.
Instead, simply list them in a `.gitignore` file and Git will ignore them automatically.
To save time, you can start with a `.gitignore` template tailored to your language or tool stack.

Don’t worry about branches, about remote hosting (GitHub/GitLab), or strict commit or code conventions on day one.
You can implement all of that later on.
If you have a local repo and you commit regularly, you have a safety net to track changes, test new ideas and roll back code whenever necessary.

Importantly, now that you know how to handle software environments, always commit your project configuration files and lockfiles (e.g. `pixi.toml`, `pyproject.toml`, `pixi.lock`, `uv.lock`, `renv.lock`, etc).
Tracking these alongside your scripts guarantees that anyone (including your future self!) can recreate your exact computational setup at any commit in the repository's history.

As your project evolves, you'll need a way to document your observations, to-do lists, etc. There are several ways to do this:

- By committing files like a `TODO.md`
- With an electronic lab notebook, though your observations will live in a completely separate space from your code
- Using the "issues" of your specific hosting platform (but these won't be portable like the rest of the repo)
- With a system involving a knowledge base (e.g. Notion, Obisidian)

If you need a `git` refresher: check out [this guide](https://rogerdudler.github.io/git-guide/)!

## Integrated Development Environments (IDEs)

There are many editors out there to help you interact with your code and manage files.
All should support syntax highlighting, interactive debugging, and variable/data frame inspection.
I've personally used [VS Code](https://code.visualstudio.com/) and [Positron](https://positron.posit.co/), a VS Code fork for data science.

I feel I lack control over some of the settings when using either.
VS Code being older, it feels more stable for some things.
Instead, I like better the approach to interactive coding in Positron.
My colleagues working in R are gradually moving from RStudio to Positron, the latter being the modern successor to the former.

For teaching purposes, I've found that [Spyder](https://docs.spyder-ide.org/current/index.html) is a lightweight option that's well suited for beginners.

And if you prefer working entirely in a terminal-based editor, there’s always [Neovim](https://neovim.io/).
While very powerful, terminal-based editors usually have a much steeper learning curve.

## Practical use of environments 

Depending on your local setup, you might find that you can write and execute code in a script or Jupyter notebook without ever creating an isolated environment or explicitly installing packages.
We strongly discourage this, as it bypasses the core practices of scientific reproducibility and environment isolation.

Even when you create a dedicated environment and install your primary packages, indirect dependencies (transitive dependencies) can still drift if unconstrained.
While lockfiles guarantee environment stability across machines, it is also good practice to document or log the explicit package versions used at the beginning of your analysis scripts or notebooks
(e.g., checking `__version__` in Python or printing `sessionInfo()` in R).

How your environments are initialized and recognized depends on your chosen IDE and environment manager:

- Activation: some env managers require activating environments in the terminal prior to installing packages or running scripts.
  Modern workspace tools like Pixi manage dependencies natively within the project root without global activation commands.
- IDE interpreter discovery: most IDEs (like VS Code, Positron, or PyCharm) will automatically scan your project directory or workspace configuration files (`pixi.toml`, `pyproject.toml`, or `environment.yml`) and prompt you to select the appropriate interpreter.
  In some cases, this step may be finicky.
- Jupyter kernel registration: if you use Jupyter notebooks inside an isolated environment, make sure the environment includes the necessary kernel bindings (`ipykernel` for Python, IRkernel for R).

As with most tooling, there is a balance between automatic, out-of-the-box IDE convenience and explicit, manual control over your runtime environment.