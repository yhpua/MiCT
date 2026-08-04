## R CMD check results

0 errors | 0 warnings | 1 note

* checking for future file timestamps ... NOTE
  unable to verify current time

This appears to be an environment-specific timestamp verification note on my
Windows machine. No package files were reported as having future timestamps.

## Resubmission

This is a resubmission. I have addressed the requested issues:

* Expanded acronyms in the DESCRIPTION field.
* Replaced `\dontrun{}` with `\donttest{}` where examples are executable but
  potentially slow.
* Removed internal code that modified `.GlobalEnv` or `.Random.seed`.
  Reproducibility is now handled by users calling `set.seed()` before functions
  involving simulation or bootstrapping.
* Updated the license declaration to `License: MIT + file LICENSE`.
* Updated the LICENSE file so that it contains only the required `YEAR:` and
  `COPYRIGHT HOLDER:` lines.

## Downstream dependencies

This is a new CRAN submission, so there are no reverse dependencies.
