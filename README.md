[RIT Linux Wiki](https://wiki.ritlug.com)
============================================================

### [_wiki.ritlug.com_](https://wiki.ritlug.com)

[![License: CC BY-SA 4.0](https://img.shields.io/badge/License-CC%20BY--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-sa/4.0)

A wiki supporting RIT students who use Linux.


## About

This repository is a collection of various information on how to access
university resources at RIT using Linux Operating Systems and Open Source
Software. This wiki has been modified from the 
[UNICEF Inventory](https://github.com/UNICEF/inventory) project.


## Development usage

### Local Install
1. Make sure you have ruby and rubygems installed per the [jekyll docs](https://jekyllrb.com/docs/installation/)
2. install bundler with `gem install bundler` (you may also need to install jekyll this way, I'm not sure)
3. Use `bundle install` followed by `bundle exec jekyll serve` to serve the project locally for development.

### Docker install
The `./start_devel_env.sh` script creates a docker environment with everything already set up for building and serving a local copy of the repository. After running, your local webserver will be visible at `localhost:4000` in your browser

## Legal

Licensed under [Creative Commons Attribution-ShareAlike 4.0 International](https://creativecommons.org/licenses/by-sa/4.0).
Not a lawyer?
This basically means you are free to:

* **Share**:
  Copy and redistribute the material in any medium or format
* **Adapt**:
  Remix, transform, and build upon the material for any purpose, even commercially

Provided you meet the following terms:

* **Attribution**:
  You must give appropriate credit, provide a link to the license, and indicate if changes were made.
  You may do so in any reasonable manner, but not in any way that suggests the licensor endorses you or your use.
* **ShareAlike**:
  If you remix, transform, or build upon the material, you must distribute your contributions under the same license as the original.
* **No additional restrictions**:
  You may not apply legal terms or technological measures that legally restrict others from doing anything the license permits.
