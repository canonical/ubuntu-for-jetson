# Ubuntu for Jetson Read The Docs

*Public documentation for Ubuntu users on Jetson Orin development kits*

## Description

This repo is based on Canonical's RTD starter pack and includes:

* A bundled [Sphinx] theme, configuration, and extensions
* Support for both reStructuredText (reST) and MyST Markdown
* Build checks for links, spelling, and inclusive language
* Customisation support layered over a core configuration

See the full documentation: https://canonical-starter-pack.readthedocs-hosted.com/

### Building documentation

* [Official Ubuntu for Jetson RTD] is built from the main branch
* Documentation builds are monitored and administrated from the [RTD WebUI]

## Structure

This section outlines the structure of this repository, and some key files.

### `docs/`

This directory contains the documentation itself.

To view it in your browser, navigate to this directory and type `make run`.

### `.github/workflows/`

This directory contains files used for documentation build checks via GitHub's CI.
Any contributor to this project is encouraged to follow this [Set-up page] in order 
to run local tests before pushing a commit.

<!--Links-->

[Official Ubuntu for Jetson RTD]: https://canonical-ubuntu-for-jetson.readthedocs-hosted.com/en/latest/
[RTD WebUI]: https://app.readthedocs.com/projects/canonical-ubuntu-for-jetson/
[Sphinx]: https://www.sphinx-doc.org/
[Set-up page]: https://canonical-starter-pack.readthedocs-hosted.com/latest/tutorial/set-up/
