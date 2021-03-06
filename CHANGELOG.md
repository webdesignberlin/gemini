# Changelog

## Dev

* Re-add `tolerance` setting, which now sets max allowed
  CIEDE2000 difference between image colors. It is now
  configurable on 3 levels:
  - `tolerance` option in config
  - `setTolerance` method for a suite
  - optional parameter to `capture` method:
    ```javascript
    suite.capture('name', {tolerance: 20}, function() {});
    ```
* Programmatic API method `buildDiff` is now deprecated: it 
  has access only to global tolerance, so diff it produces might not show
  exactly what caused test to fail. Method is kept for backward compatibility, 
  but users of a programmatic API now encouraged to use `saveDiffTo` method of
  test result (passed to reporter) instead.
* Warn if coverage for CSS file cannot be calculated due to
  same-origin policy (@SevInf)

## 0.9.8 - 2015-02-11

* Work correctly if WebDriver implementation returns screenshot with
  a browser chrome. This is done via calibration step after first launch
  of the browsers (@scf2k).
* Add ability to ignore certain elements when comparing screenshots.
  Use `suite.ignoreElements(selector1, selector2, ...)` to specify
  the selectors to ignore (@SevInf).
* Add `tap` action for touch devices (@scf2k).
* `gemini` is now works correctly if page changes during the test (@scf2k).
* Correctly handle missing timeout in waitForElementXXX (@SevInf).
* Correctly report image path in `gather` API (@SevInf).

## 0.9.7 - 2015-02-09

* Add new wait methods (@SevInf):
  - `waitForElementToShow`
  - `waitForElementToHide`
  - `waitForJSCondition`

## 0.9.6 - 2015-01-27

* Work on a pages that modify `Array.prototype` (@SevInf).

## 0.9.5 - 2014-12-10

* Works on Windows again (@SevInf).

## 0.9.4 - 2014-12-03

* Add `sendFile` action which now should be used instead of
  `sendKeys` to set a file to `input[type=file]` elements (@SevInf).
* Correctly parse box-shadow in IE9 (@scf2k).
* Fail only single test if reference image is not found (@SevInf).

## 0.9.3 - 2014-11-07

* Coverage generator now follows symlinks while resolving the paths to CSS
  files. So the final report will have real paths but not pointing to symlinks.

## 0.9.2 - 2014-11-07

* Move the coverage HTML templating code to separate module (gemini-coverage).
* Detailed error message when capture region exceeds screenshot
area. Such error also will now fail only single state instead of
a whole testing process (@SevInf).

## 0.9.1 - 2014-10-23

* Ignore `@keyframes` at-rule while collecting coverage (@scf2k).

## 0.9.0 - 2014-10-22

* Replace `GraphicsMagick` with lightweight `png-img` library (@j0tunn).
  As for this version, `gemini` no longer requires any external tool for
  image processing.
* CSS coverage now supports source maps (@scf2k).
  If your CSS has the sourcemap, coverage report will show original files.

  You can use `sourceRoot` option to tell `gemini` where are your sources
  located on a filesystem.
* CSS coverages statistics will now also be written to `coverage.json`
  file (@scf2k).
* Add `coverageExclude` option to allow exclude certain files from coverage
  report (@scf2k).
* Add index page to coverage report and improve appearance of a the coverage
  pages (@scf2k).
* Restructure documentation and add quick start guide in russian and english
  (@jk708).

## 0.8.2 - 2014-10-06

* Reset cursor position before reloading the page.
When resetting it just after page load, old cursor position
may trigger some transitions which will not be finished before
first screenshot.

## 0.8.1 – 2014-09-30

* `test` command will exit with correct codes

## 0.8.0 - 2014-09-30

* New image comparison algorithm is implemented:
  - `gm compare` replaced with custom diff. For now, GraphicsMagick
  is still required for other image manipulations.
  - `tolerance` setting is removed in favor of "strict mode": by default,
  only noticable changes (according to ciede2000 metric) will be treated
  as failure, to treat all changes that way user can enable
  `strictComparison` option.
  - in case if some element in focus during test can potentially have
  blinking caret displayed, diff will try to ignore caret.

* Remove built-in TeamCity reporter. If you really want
it, you can adapt 0.7.x reporter to the current version
of `gemini` and publish it in separate package.
* Remove legacy ability to specify browsers as array which
  was deprecated since 0.4.0.
* Add experimental [programmatic API](doc/programmatic-api.md).
* Add `--grep` option to `gather` and `test` commands, which
  allows to execute only suites, matching the pattern (@scf2k).
* Show required dimensions in error message when origin does
  not fir to full-page screenshot (@scf2k)

## 0.7.0 - 2014-09-15

* Show meaningful error when capture area origin
  does not fit to full-page screenshot. This change
  can break some of your tests (@scf2k).
* Add experimental CSS-coverage report. Enable
  with `--coverage` CLI flag or `coverage: true` in
  config (@scf2k).
* Add ability to override `gridUrl`,`rootUrl`, `screenshotsDir`,
  `debug` config options by their respective CLI options or
  environment variables (@arikon).
* Add config option `windowSize` to specify default
  size of the browser windows (@scf2k).
* Add action `setWindowSize` to specify browser
  window size during the tests (@scf2k).


## 0.6.5 - 2014-09-12

Was republished as 0.7.0 due to a breaking change.

## 0.6.4 - 2014-08-13

* Add command line completion (@unlok).
* Show an error, when config file has unknown
  options.
* Add `focus(element)` action (@arikon).

## 0.6.3 - 2014-08-05

* Allow to use empty string with `sendKeys`. It can be used
  to focus on an element without changing states.

## 0.6.2 - 2014-08-01

* Reset mouse position for each suite. Previously, cursor
  may stay at the position left from a previous suite and
  some elements was captured with hover effect when there
  shouldn't be any.

## 0.6.1 - 2014-07-30

* Fix incorrect capture region rounding, causing bottom row
  of the element to be cropped sometimes.
  

## 0.6.0 - 2014-07-28

* `:before` and `:after` pseudo-elements outline and shadow are now
  taken into account when calculating capture region(@incorp).

  This change can break some of your tests. Re-gather reference images
  to fix the problem.
* Added `tolerance` config option which can be used to specify maximum
  error rate before images will be treated as unequal.
  Default tolerance to 0. This is stricter then previous versions:
  now every, even slightest difference between reference and current
  images will fail tests.

  Set `tolerance` to 0.001 in `.gemini.yml` to restore 0.5.x behavior.
* Coordinates of capture region are now rounded to capture maximal area.
  Previously, border pixels could be cropped, due to rounding error.
  This change can break some of your tests. Re-gather reference images
  to fix the problem.

* Added ability to change diff highlight color in config file.
* Change diff highlight style 

* Global installations of `gemini` now runs local one, if available.

## 0.5.0 - 2014-07-17

* Browsers are now launched once for each run (previously, they were
  launched once per suite). This greatly reduces total tests run time,
  but can break some of your code, i.e. each `mouseDown` should always
  be closed by `mouseUp`.
  Previously, this was not required if `mouseDown` was used once for
  suite.
  It will show warning if versions of the modules does not match.
* `flat` reporter replaces `tree`. Tree reporter can not work with new
  browser launch model.
* Add `parallelLimit` option that allows to limit number of browsers
 run in parallel.
* Add `suite.after()` which can be used to perform some action after all
  of the states without taking screenshot.
* Unknown errors, returned by Selenium have more detailed report.
* Fix `--version` option.

## 0.4.2 - 2014-06-25

* Fix missing images in html report.

## 0.4.1 - 2014-06-19

* Correctly detect crop region in Firefox

## 0.4.0 - 2014-06-18

* Crop region for screenshots is calculated via client script inside
  browser instead of `gemini`. This allows to issue fewer requests to
  Selenium Server speeding up total tests run time. This feature breaks
  compatibility with old browsers (`IE` < 9).
* New config format, which allows to specify full set of capabilities
  for browsers:

  ```yaml
  browsers:
    phantomjs: phantomjs
    opera12:
        browserName: opera
        version: '12.06'
        platform: 'WINDOWS'
    firefox28:
        browserName: firefox
        version: '28.0'
    firefox27:
        browserName: firefox
        version: '27.0'
  ```
* Correctly capture screenshots of regions out of initial browser
  viewport in  browsers, that can't capture full page (`Opera` and
  `Chrome` at the time of writing).
* `outline-width` of an elements is now also taken into account when 
  calculating crop region.
* Add `debug` options to config file. If set to `true`, `gemini` will
  print debug logs to STDOUT. (@arikon).
* Add `http` section to config file which allows to configure HTTP
  timeout (`http.timeout`) retry count (`http.retries`) and delay
  between retries (`http.retryDelay`). (@arikon).
* Asynchronous errors stacktraces in browser actions (such as not found
  element) will point to users code.
* More HTTP requests to Selenium will run in parallel speeding up
  `gather`/`test` commands (@arikon).
* If `gemini` is run without subcommand, help text will be shown.

## 0.3.4 - 2014-05-28

* Enhanced html report:
  - suites are now collapsible;
  - all but failed suites are collapsed by default;
  - buttons to expand all, collapse all and expand only errors are added;
  - stats of total numbers of tests run, failed, succeeded and skipped
    are shown at the top.

## 0.3.3 - 2014-05-19

* Allow to use multiple reporters in `test` command.
* Throw error when creating multiple suites of the same name within
  the same parent.
* Throw error when creating multiple states of the same name within
  the suite.
* Throw error when creating suite that will be unable to run (
  has states and hasn't url or capture region);
* Check argument types of `SuiteBuilder` methods.
* Check argument types of all actions methods.
* Shorter stacktraces for invalid elements  errors.
* Correctly handle offsets in `mouseMove` actions.
* Fix error when `gridUrl` was required even if there is only
  `phantomjs` browser.

## 0.3.2 - 2014-05-15

* Allow to override `gridUrl` and `rootUrl` settings with cli
  options `--grid-url` and `--root-url`.
* Correctly report error, when wrong argument passed to an action.

## 0.3.1 - 2014-05-13

* Ability to set additional capabilities for all browsers, using
  `capabilities` option in `.gemini.yml`:

  ```yaml
  capabilities:
    option1: value,
    option2: value
  ```
* Non-existent directories, passed to `gather` and `test` commands will
  be filtered out
* If fatal error occurs, `gemini` will always exit with 1 status code
* If test fails or state error occurs, `gemini` will always exit with 2
  status code.
* When `gemini` is unable to launch browser, more clearer error message
  will be displayed.

## 0.3.0 -  2014-04-30

* Elements to take screen shots of and elements to perform action
  on are now defined differently. `setElements` and `setDynmaicElements`
  methods removed.

  New way to define elements for screenshot:

  ```javascript
  suite.setCaptureElements('.selector1', '.selector2', ...)
  ```

  Or using array:

  ```javascript
  suite.setCaptureElements(['.selector1', '.selector2', ...])
  ```


  To get element to perform action on, you can now pass
  selectors directly to the actions:

  ```javascript
  suite.capture('state', function(actions, find) {
      actions.click('.button');
  });
  ```

  To avoid multiple lookups for the same element you can use 
  `find` function which is now passed to the state callback:

  ```javascript
  suite.capture('state', function(actions, find) {
      var button = find('.button');
      actions.mouseMove(button);
             .click(button);
  });
  ```

* Add `suite.before(function(action, find))` which can be used to
  perform some actions before the first state. Context is shared
  between before hook and all of suite's state callbacks.

  You can use `before` to look for element only once for the state:

  ```javascript
  suite.before(function(actions, find) {
      this.button = find('.buttons');
  })
  .capture('hovered', function(actions, find) {
      actions.mouseMove(this.button);
  })
  .capture('pressed', function(actions, find) {
      actions.mouseDown(this.button);
  });
  ```

  Or to perform some actions before first state without taking
  screenshot:

  ```javascript
  suite.before(function(actions, find) {
      actions.click('.button');
  });
  ```

* Add `suite.skip()` method which allows to skip suite in some set of
  browsers:

  - `suite.skip()` - skip in all browsers.
  - `suite.skip('chrome')` - skip in all versions of Chrome.
  - `suite.skip({name: 'chrome', version: '33.0'})` - skip in Chrome 33.0
  - `suite.skip(['chrome', 'opera'])` - skip in Chrome and Opera

* Public API now has constants for special keys to use in `sendKeys` actions
  (i.e. `gemini.CONTROL` for `CTRL` key).

## 0.2.1 - 2014-04-23

* Fix a bug with incorrect reference to the suite in states. Because of
  this bug dynamic elements was not updated properly.

## 0.2.0 - 2014-04-22

* New test suites API.  Plans are replaced by test suites defined by explicit call.

  Old version:

  ```javascript
  module.exports = function(plan) {
      plan.setName('some name')
          .setElements(...)
          .setDynamicElements(...)
          .capture(...)
          
  };
  ```

  New API:

  ```javascript
  var gemini = require('gemini');

  gemini.suite('some name', function(suite) {
      suite.setElements(...)
          .setDynamicElements(...)
          .capture(...)
          
  };

  ```

  Suites also can be nested. In this case, child suite inherits all properties 
  from the parent.

  ```javascript
  gemin.suite('parent', function(suite) {
      gemini.suite('child', function(child) {
          gemini.suite('grandchild', function(grandchild) {

          });
      });
  });
  ```
* `.reload()` method is removed. Use nested suite if you need to reload browser.

* Added action to run any JS code in browser context:

  ```javascript
  actions.executeJS(function(window) {
      window.alert('Hello!');
  });
  ```

* `sendKeys` action can optionally take an receive an element to send keys to.

  ```javascript
  actions.sendKeys(elements.someInput, 'hello');
  ```
* Added ability to specify browser version in `.gemini.yml`

  ```yaml
  browsers:
    - {name: 'phantomjs', version: '1.9'}
  ```
* Tree reporter is now used for `gather` command.

## 0.1.1 - 2014-04-08

* `phantomjs` always starts maximized. This fixes the error, when some shadows didn't fit in crop rectangle.
* Action on dynamic element that does not currently exists causes non-fatal error. Such error will fail only one state, the rest will continue running.
* `gather` command now reports browser name.
* browsers are always closed, even if there was an error.

## 0.1.0 - 2014-03-27

* Initial release
