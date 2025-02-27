# @promster/express

## 7.0.6

### Patch Changes

- [#1006](https://github.com/tdeekens/promster/pull/1006) [`8df4fe6`](https://github.com/tdeekens/promster/commit/8df4fe6a4cd6ff86260ce546411200d7e4d98802) Thanks [@tdeekens](https://github.com/tdeekens)! - chore: update deps

- Updated dependencies [[`58ffa97`](https://github.com/tdeekens/promster/commit/58ffa977892fa97f47fdbf208f76bd0ef2b8b2ee), [`8df4fe6`](https://github.com/tdeekens/promster/commit/8df4fe6a4cd6ff86260ce546411200d7e4d98802)]:
  - @promster/metrics@9.1.6

## 7.0.5

### Patch Changes

- [`5d4842e`](https://github.com/tdeekens/promster/commit/5d4842e5d60136effcff1905b06b6bae06063c3b) Thanks [@tdeekens](https://github.com/tdeekens)! - Update all dependencies

- Updated dependencies [[`5d4842e`](https://github.com/tdeekens/promster/commit/5d4842e5d60136effcff1905b06b6bae06063c3b)]:
  - @promster/metrics@9.1.5

## 7.0.4

### Patch Changes

- [#865](https://github.com/tdeekens/promster/pull/865) [`abc0df1`](https://github.com/tdeekens/promster/commit/abc0df1a08e46c36659165938b62b3764501cf6d) Thanks [@tdeekens](https://github.com/tdeekens)! - Fix timing to not truncate fractional numbers.

- Updated dependencies [[`abc0df1`](https://github.com/tdeekens/promster/commit/abc0df1a08e46c36659165938b62b3764501cf6d)]:
  - @promster/metrics@9.1.4

## 7.0.3

### Patch Changes

- [#848](https://github.com/tdeekens/promster/pull/848) [`0e033fa`](https://github.com/tdeekens/promster/commit/0e033fa7ac414dc3692ceedf54eca6cfc2370ff0) Thanks [@tdeekens](https://github.com/tdeekens)! - Update dependencies

- Updated dependencies [[`defcae7`](https://github.com/tdeekens/promster/commit/defcae775e7280a5746db4c2e75226353d7bdf40), [`0e033fa`](https://github.com/tdeekens/promster/commit/0e033fa7ac414dc3692ceedf54eca6cfc2370ff0)]:
  - @promster/metrics@9.1.3

## 7.0.2

### Patch Changes

- [#794](https://github.com/tdeekens/promster/pull/794) [`06cf42b`](https://github.com/tdeekens/promster/commit/06cf42b46bd216f6c3c182452d6badfe1029c7d3) Thanks [@tdeekens](https://github.com/tdeekens)! - Fix package names from `yarn@3` migration

- Updated dependencies [[`06cf42b`](https://github.com/tdeekens/promster/commit/06cf42b46bd216f6c3c182452d6badfe1029c7d3)]:
  - @promster/metrics@9.1.1

## 7.0.1

### Patch Changes

- [#791](https://github.com/tdeekens/promster/pull/791) [`7f44d2e`](https://github.com/tdeekens/promster/commit/7f44d2e16b506cb53ff0009f4f74a8ccef192ce0) Thanks [@tdeekens](https://github.com/tdeekens)! - Refactor to use `process.hrtime.bigint()` over `process.hrtime()`.

  The [Node.js documentation](https://nodejs.org/api/process.html#processhrtimetime) lists `process.hrtime([time])` as a [legacy feature](https://nodejs.org/api/documentation.html#stability-index). It is no longer recommended for use, and should be avoided. As such it is recommended to use `process.hrtime.bigint()` which works slightly different.

  This change is backwards compatible. A new `timing` module was added to `@promster/metrics` which encapsulates timings so you never have to worry about the details.

  ```diff
  import { getRequestRecorder } = require('@promster/express');
  +import { timing } = require('@promster/express');

  +const requestTiming = timing.start();
  -const requestTiming = process.hrtime();

  recordRequest(requestTiming);
  ```

  We recommend to change all explicit calls to `process.hrtime()` with calls to `timing.start()` over time. The `recordRequest` function will continue to accept a `[number, number]` as the return of `process.hrtime()` so you can change code as you work with it.

- Updated dependencies [[`7f44d2e`](https://github.com/tdeekens/promster/commit/7f44d2e16b506cb53ff0009f4f74a8ccef192ce0)]:
  - @promster/metrics@9.1.0

## 7.0.0

### Major Changes

- [#778](https://github.com/tdeekens/promster/pull/778) [`e5658c7`](https://github.com/tdeekens/promster/commit/e5658c7189d09071a21c299b78423e0782392c07) Thanks [@tdeekens](https://github.com/tdeekens)! - This major change removes the support for the `accurancies` option. Historically, `promster` first supported milliseconds. Then learning that seconds is the default time interval in the Prometheus community. The `accuracies` option was added to enable a soft migration from milliseconds to seconds not to always support both time intervals.

  In case you haven't migrated your metrics to seconds and don't want to loose data we recommend to have the same metric with two names using the `metricNames` option ending in `*_seconds` and `*_milliseconds`. This way you only need to change the time interval in e.g. Grafana when hitting the time when the library was updated. Then migrate all dashboards and recording rules and eventually remove the `*_milliseconds` `metricName` option again.

  If you would like to read more about the reasoning of this decision head [here](https://www.robustperception.io/who-wants-seconds).

### Minor Changes

- [#771](https://github.com/tdeekens/promster/pull/771) [`dfc14a3`](https://github.com/tdeekens/promster/commit/dfc14a3d8ec309723c6b904949e506c63c04c3c0) Thanks [@tdeekens](https://github.com/tdeekens)! - Allow passing a `disableGcMetrics` boolean option to disable Garbage Collection metrics.

### Patch Changes

- [#772](https://github.com/tdeekens/promster/pull/772) [`402c8c0`](https://github.com/tdeekens/promster/commit/402c8c01a3dfa605d47e0f974ac554d65c237d50) Thanks [@tdeekens](https://github.com/tdeekens)! - Refactor to split metric types into `gcMetrics` and `httpMetrics` to empower future metric types.

* [#778](https://github.com/tdeekens/promster/pull/778) [`e5658c7`](https://github.com/tdeekens/promster/commit/e5658c7189d09071a21c299b78423e0782392c07) Thanks [@tdeekens](https://github.com/tdeekens)! - Fixes that not always a `{ req, res }` object was passed to all normalizers in all plugins by making the typing more explicit.

- [#769](https://github.com/tdeekens/promster/pull/769) [`5e67884`](https://github.com/tdeekens/promster/commit/5e6788481fee15479c6505d3475ab716f7378f33) Thanks [@tdeekens](https://github.com/tdeekens)! - Refactor to colocate environment detection

* [#777](https://github.com/tdeekens/promster/pull/777) [`9d6a2ee`](https://github.com/tdeekens/promster/commit/9d6a2ee81ea3e40fac3e788d9942f66c868670a5) Thanks [@tdeekens](https://github.com/tdeekens)! - refactor: to use yarn@3

- [#774](https://github.com/tdeekens/promster/pull/774) [`e537f71`](https://github.com/tdeekens/promster/commit/e537f719f6905a3619b9bbe8d36236df069c64f7) Thanks [@tdeekens](https://github.com/tdeekens)! - refactor(express): to integration test

- Updated dependencies [[`d49c30f`](https://github.com/tdeekens/promster/commit/d49c30f0004e0f4883ac356598ddf82b87b7c9d6), [`402c8c0`](https://github.com/tdeekens/promster/commit/402c8c01a3dfa605d47e0f974ac554d65c237d50), [`e5658c7`](https://github.com/tdeekens/promster/commit/e5658c7189d09071a21c299b78423e0782392c07), [`e5658c7`](https://github.com/tdeekens/promster/commit/e5658c7189d09071a21c299b78423e0782392c07), [`1b84464`](https://github.com/tdeekens/promster/commit/1b8446455324e4ceefb4b032eddb43e9b8bb63d2), [`548c56e`](https://github.com/tdeekens/promster/commit/548c56e1ec7ccaa5b1aca71d33db65ad91e64f3c), [`5e67884`](https://github.com/tdeekens/promster/commit/5e6788481fee15479c6505d3475ab716f7378f33), [`dfc14a3`](https://github.com/tdeekens/promster/commit/dfc14a3d8ec309723c6b904949e506c63c04c3c0), [`9d6a2ee`](https://github.com/tdeekens/promster/commit/9d6a2ee81ea3e40fac3e788d9942f66c868670a5)]:
  - @promster/metrics@9.0.0

## 6.0.0

### Major Changes

- [#758](https://github.com/tdeekens/promster/pull/758) [`f02e6fe`](https://github.com/tdeekens/promster/commit/f02e6fe9950070d7df3cc0854f407cabe10d2aea) Thanks [@tdeekens](https://github.com/tdeekens)! - We droped support for Node.js `< 14` via the `engines` field of the respective `package.json` files.

  We didn't make any changes to our code to prevent e.g. Node.js v9 to work but will not claim to officially support it any longer.

### Patch Changes

- [#762](https://github.com/tdeekens/promster/pull/762) [`92cdf28`](https://github.com/tdeekens/promster/commit/92cdf281ff57f901835f14d474ae9cc1fb6fe2b2) Thanks [@tdeekens](https://github.com/tdeekens)! - Update dependencies

- Updated dependencies [[`f02e6fe`](https://github.com/tdeekens/promster/commit/f02e6fe9950070d7df3cc0854f407cabe10d2aea), [`7e7a1c6`](https://github.com/tdeekens/promster/commit/7e7a1c615e493217dfed00966f89880db9322485), [`92cdf28`](https://github.com/tdeekens/promster/commit/92cdf281ff57f901835f14d474ae9cc1fb6fe2b2), [`92cdf28`](https://github.com/tdeekens/promster/commit/92cdf281ff57f901835f14d474ae9cc1fb6fe2b2)]:
  - @promster/metrics@8.0.0

## 5.1.3

### Patch Changes

- [#755](https://github.com/tdeekens/promster/pull/755) [`84435d4`](https://github.com/tdeekens/promster/commit/84435d46f295ad2e80d40328c95852a1e7beb0de) Thanks [@tdeekens](https://github.com/tdeekens)! - Update dependencies and release all packages

- Updated dependencies [[`84435d4`](https://github.com/tdeekens/promster/commit/84435d46f295ad2e80d40328c95852a1e7beb0de)]:
  - @promster/metrics@7.0.4

## 5.1.2

### Patch Changes

- [#740](https://github.com/tdeekens/promster/pull/740) [`08b8029`](https://github.com/tdeekens/promster/commit/08b802983337ca55a95aad887abbf43e6469830b) Thanks [@tdeekens](https://github.com/tdeekens)! - Fix ontent-length mentric to default to 0

* [`a24f38f`](https://github.com/tdeekens/promster/commit/a24f38ff070220a28da5bf64d7d9e3a3167d1152) Thanks [@tdeekens](https://github.com/tdeekens)! - Update all dependencies including gc-stats

* Updated dependencies [[`a24f38f`](https://github.com/tdeekens/promster/commit/a24f38ff070220a28da5bf64d7d9e3a3167d1152)]:
  - @promster/metrics@7.0.2

## 5.1.1

### Patch Changes

- [`cbc12c7`](https://github.com/tdeekens/promster/commit/cbc12c76ce1414f08f914ceb5ecd9747d497b71d) Thanks [@tdeekens](https://github.com/tdeekens)! - Update dependencies and added spell check as a GitHub Action

- Updated dependencies [[`cbc12c7`](https://github.com/tdeekens/promster/commit/cbc12c76ce1414f08f914ceb5ecd9747d497b71d)]:
  - @promster/metrics@7.0.1

## 5.1.0

### Minor Changes

- [#705](https://github.com/tdeekens/promster/pull/705) [`e2a1595`](https://github.com/tdeekens/promster/commit/e2a15959c4f11191093a0f0fa0e472434207f020) Thanks [@tdeekens](https://github.com/tdeekens)! - Add content length metric for both requests and responses. Both are recorded as Prometheus histograms under the name `http_request_content_length_bytes`
  and `http_response_content_length_bytes`.

### Patch Changes

- Updated dependencies [[`e351e91`](https://github.com/tdeekens/promster/commit/e351e91b68ebf3bb13e70e5fa4925aaa96343344), [`e2a1595`](https://github.com/tdeekens/promster/commit/e2a15959c4f11191093a0f0fa0e472434207f020), [`e351e91`](https://github.com/tdeekens/promster/commit/e351e91b68ebf3bb13e70e5fa4925aaa96343344)]:
  - @promster/metrics@7.0.0

## 5.0.3

### Patch Changes

- [#620](https://github.com/tdeekens/promster/pull/620) [`ec2a8f8`](https://github.com/tdeekens/promster/commit/ec2a8f83a94bbe63360cf7027eeba92895315a19) Thanks [@tdeekens](https://github.com/tdeekens)! - refactor: improve typings (not exposed part)

* [#618](https://github.com/tdeekens/promster/pull/618) [`bbab9ca`](https://github.com/tdeekens/promster/commit/bbab9cad6a3484e4894d159267d62e54e202812a) Thanks [@tdeekens](https://github.com/tdeekens)! - chore: update deps

* Updated dependencies [[`ec2a8f8`](https://github.com/tdeekens/promster/commit/ec2a8f83a94bbe63360cf7027eeba92895315a19), [`bbab9ca`](https://github.com/tdeekens/promster/commit/bbab9cad6a3484e4894d159267d62e54e202812a)]:
  - @promster/metrics@6.0.2

## 5.0.2

### Patch Changes

- [#601](https://github.com/tdeekens/promster/pull/601) [`17a24dc`](https://github.com/tdeekens/promster/commit/17a24dc0d735478001524c853b9f54f862153852) Thanks [@tdeekens](https://github.com/tdeekens)! - Update dependencies and apply eslint rule suggestions.

- Updated dependencies [[`17a24dc`](https://github.com/tdeekens/promster/commit/17a24dc0d735478001524c853b9f54f862153852)]:
  - @promster/metrics@6.0.1

## 5.0.1

### Patch Changes

- Updated dependencies [[`daf8605`](https://github.com/tdeekens/promster/commit/daf86055e64cb420c83dbc7abbcd5024d449c53f)]:
  - @promster/metrics@6.0.0

## 5.0.0

### Major Changes

- [`0eb64ca`](https://github.com/tdeekens/promster/commit/0eb64cac9a4a51dab1a556f46c97a2a5542bcc88) [#529](https://github.com/tdeekens/promster/pull/529) Thanks [@tdeekens](https://github.com/tdeekens)! - # Introduction

  refactor: to use preconstruct for building

  Prior TypeScript was used to build bundles. In all this should not affect consumers of this library. Under the hood preconstruct uses rollup and babel which is now instructed to build for Node.js v12 using the preset-env preset.

  # Breaking Change

  This release can _potentially_ be breaking for you. We want to respect semantic versioning and follow it strictly.

  While migrating to preconstruct the `version` exports had to be removed as preconstruct's rollup will not resolve them. If you relied on this value you should either load the `package.json` of each module yourself or drop the usage.

  ```diff
  - const { version } = require('@promster/server');
  + const { version } = require('@promster/server/package.json');
  ```

### Patch Changes

- [`bdf75de`](https://github.com/tdeekens/promster/commit/bdf75dec8d0ce6be65ecccf5963f348e1a0a96b3) [#531](https://github.com/tdeekens/promster/pull/531) Thanks [@tdeekens](https://github.com/tdeekens)! - fix: update dependencies

- Updated dependencies [[`bdf75de`](https://github.com/tdeekens/promster/commit/bdf75dec8d0ce6be65ecccf5963f348e1a0a96b3), [`0eb64ca`](https://github.com/tdeekens/promster/commit/0eb64cac9a4a51dab1a556f46c97a2a5542bcc88)]:
  - @promster/metrics@5.0.0

## 4.1.15

### Patch Changes

- [`a0dc407`](https://github.com/tdeekens/promster/commit/a0dc407f1ceca1129942b30b7149a0d4e2b5fef8) [#500](https://github.com/tdeekens/promster/pull/500) Thanks [@natoehv](https://github.com/natoehv)! - changed TApp typing

## 4.1.14

### Patch Changes

- [`4c3af06`](https://github.com/tdeekens/promster/commit/4c3af06bc0b65eb067f195591e48839e632375bd) [#483](https://github.com/tdeekens/promster/pull/483) Thanks [@tdeekens](https://github.com/tdeekens)! - chore: update dependencies

- Updated dependencies [[`4c3af06`](https://github.com/tdeekens/promster/commit/4c3af06bc0b65eb067f195591e48839e632375bd)]:
  - @promster/metrics@4.1.13

## 4.1.13

### Patch Changes

- [`158e7b9`](https://github.com/tdeekens/promster/commit/158e7b9af01133db54376bb96dbdccdd96bfa7a3) Thanks [@tdeekens](https://github.com/tdeekens)! - Dependency updates across all packages

- Updated dependencies [[`158e7b9`](https://github.com/tdeekens/promster/commit/158e7b9af01133db54376bb96dbdccdd96bfa7a3)]:
  - @promster/metrics@4.1.12

## 4.1.12

### Patch Changes

- [`7853faa`](https://github.com/tdeekens/promster/commit/7853faa4e140eaae0622fb7c66d8145d258d7f5e) [#447](https://github.com/tdeekens/promster/pull/447) Thanks [@tdeekens](https://github.com/tdeekens)! - chore: update deps

- Updated dependencies [[`7853faa`](https://github.com/tdeekens/promster/commit/7853faa4e140eaae0622fb7c66d8145d258d7f5e)]:
  - @promster/metrics@4.1.11

## 4.1.11

### Patch Changes

- [`c2b8e0d`](https://github.com/tdeekens/promster/commit/c2b8e0d472b6e31e053460d8f714eaf790a17eb9) [#358](https://github.com/tdeekens/promster/pull/358) Thanks [@tdeekens](https://github.com/tdeekens)! - Update dependencies.

* [`6f99f64`](https://github.com/tdeekens/promster/commit/6f99f644ccd2cd0b60c172968266c3ac3f76e826) [#360](https://github.com/tdeekens/promster/pull/360) Thanks [@tdeekens](https://github.com/tdeekens)! - Update to eslint v7

* Updated dependencies [[`c2b8e0d`](https://github.com/tdeekens/promster/commit/c2b8e0d472b6e31e053460d8f714eaf790a17eb9), [`6f99f64`](https://github.com/tdeekens/promster/commit/6f99f644ccd2cd0b60c172968266c3ac3f76e826)]:
  - @promster/metrics@4.1.10
