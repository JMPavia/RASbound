# RASbound 0.1.0

First version of the package.

## Functions

* `ras()`: classic RAS (biproportional matrix balancing / IPF / raking).
* `bras_freeze_sim()`, `bras_freeze_seq()`, `bras_freeze_hybrid()`:
  bounded RAS that solves the free problem, fixes violating cells at their
  bound, and repeats. Margins are matched exactly and bounds are
  respected whenever a solution is found. `bras_freeze_hybrid()` retries
  the whole problem with sequential fixation when its per-pass fallback
  fails, and is the recommended default.
* `bras_naive()`, `bras_soft()`, `bras_cap_iter()`: reference methods for
  comparison.
* `bras_calibrate_logit()`, `bras_calibrate_linear()`,
  `bras_calibrate_raking()`: wrappers around `survey::calibrate()`. They
  remove rows and columns with a zero target before calling `survey` and
  put them back as zeros afterwards.
* All functions return an object of class `bras_fit`, with `print()`,
  `summary()` and `plot()` methods.

## Diagnostics

* `fit$feasible` and `fit$diagnostics$infeasible_at` report proven
  infeasibility and the rows and columns involved.
* `fit$diagnostics$stalled` reports a stall: no bound is violated, but
  the alternating scaling has reached a fixed point that does not match
  the margins. Increasing `max_iter` or `max_outer` does not help in that
  case.
* A seed value outside its own `[lower, upper]` range is moved to the
  nearest bound, with a warning.

## Known limitations

* The feasibility check done before solving is necessary but not
  sufficient. A joint infeasibility can still appear as a stall.
* The `bras_calibrate_*()` wrappers can fail inside `survey::calibrate()`
  on large real tables. In that case use a `bras_freeze_*()` function.

## Documentation

* Vignettes: `RASbound-intro`, `RASbound-example` (a real, rounded
  57 x 57 input-output-style table) and `RASbound-diagnostics`.
