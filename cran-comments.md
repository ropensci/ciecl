## Submission ciecl 1.0.0

This is an update of ciecl (currently 0.9.8 on CRAN). Version 1.0.0 is
the first release after the package passed rOpenSci peer review
(https://github.com/ropensci/software-review/issues/765) and was
transferred to the ropensci organization. It consolidates the
user-facing improvements made during the review; there are no breaking
changes to the CRAN-published API surface.

## Summary of changes since 0.9.8

* **Typed input validation**: public functions abort with informative
  errors of class `ciecl_invalid_input` instead of base R errors.
* **Deterministic ordering** in `cie_lookup()` (explicit `ORDER BY`)
  and inclusive upper bounds in code ranges such as `"E10-E14"`.
* **Robustness fixes** in `cie10_sql()` (legitimate SELECTs starting
  with a comment) and in `cie_search()` with symbol-only text.
* Documentation site moved to <https://docs.ropensci.org/ciecl/>.

Full changelog in NEWS.md.

## Test results

* **Tests**: devtools::test() passes locally (R 4.6.0); see
  tests/testthat.
* **R CMD check --as-cran**: 0 errors | 0 warnings | 1 note on the
  submitted tarball (Windows 11 x64, R 4.6.0); the note is a CRAN
  incoming feasibility NOTE only, explained below.

### Skip ratio on CRAN (~58%)

About 58% of the tests call `skip_on_cran()`. This is deliberate
("CRAN canary" policy, documented in `tests/testthat/setup.R`): the
skipped tests rebuild the local SQLite cache (~16 s), require network
access to the WHO ICD-11 API, depend on optional Suggests packages
(comorbidity, gt), or assert timing thresholds that are unstable on
heterogeneous hardware. CRAN still runs a set of lightweight canary
tests covering the base flow (normalize -> validate -> lookup over the
bundled dataset), with no network, no optional dependencies, and no
writes outside tempdir.

### NOTEs explained

1. **CRAN incoming feasibility — possibly invalid URL (HTTP 401)**:
   https://www.bcn.cl/leychile/navegar?i=1112064 (in README.md) returns
   HTTP 401 to automated checkers due to anti-bot protection on Chilean
   government servers. The URL is valid and accessible via regular
   browsers. The same applies to the MINSAL/DEIS URLs in the
   documentation (HTTP 403 to automated tools):
   - https://deis.minsal.cl
   - https://deis.minsal.cl/centrofic/
   - https://repositoriodeis.minsal.cl

2. **Examples with elapsed time > 5s** (only under `--run-donttest`):
   Examples for `cie_search()` and `cie_lookup()` are already wrapped
   in `\donttest{}` because they trigger first-run SQLite cache
   initialization. They do not execute under standard CRAN checks;
   the NOTE is only produced when `--run-donttest` is passed
   explicitly.

## Test environments

* Local: Windows 11 x64, R 4.6.0
* GitHub Actions R-CMD-check:
  - macOS-latest (release)
  - windows-latest (release)
  - ubuntu-latest (devel, release, oldrel-1)
* R-hub (Windows, macOS, Linux)

## Downstream dependencies

This package has no reverse dependencies (verified via
`revdepcheck::revdep_check()`).
