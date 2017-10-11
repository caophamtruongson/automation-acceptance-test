# Automation Acceptance Testing

This repository/topic is one small part of this series:[「How TDD helps to build better process for software development team」](https://medium.com/@caophamtruongson/how-tdd-helps-building-better-process-for-software-development-team-318b612dbb1b)

## Index

1. [Steps](#steps)
1. [Reference](#reference)


## Steps 

### Codeception installation

`composer require "codeception/codeception" --dev`

### Acceptance Test structure initiation 

`./vendor/bin/codecept init acceptance`

You will be asked for providing where your tests will be stored, which kind of browser for testing, and url for test. For me, one by one is: tests, phantomjs, http://localhost.

### Download PhantomJS

As above, I use「PhantomJS」as browser, so「PhantomJS」need to be download to local machine and place to suitable folder.

1. Download link
    * http://phantomjs.org/download.html, current version for mac: `phantomjs-2.1.1-macosx`
1. Move package to suitable folder for centralize storage in the future
    * Move downloaded package to `/usr/local/share/phantomjs-2.1.1-macosx/`
    * Make soft link `/usr/local/bin/phantomjs` to `/usr/local/share/phantomjs-2.1.1-macosx/bin/phantomjs`
        * `ln -s /usr/local/share/phantomjs-2.1.1-macosx/bin/phantomjs /usr/local/bin/phantomjs`

### Running
    
1. Running「phantomjs」
    ```
    $ nohup phantomjs --webdriver=4444 --ignore-ssl-errors=true --ssl-protocol=tlsv1  >&- 2>&- <&- &
    [1] 12708
    ```
1. Run a test
    ```
    $ ./vendor/bin/codecept run
    Codeception PHP Testing Framework v2.3.6
    Powered by PHPUnit 5.7.22 by Sebastian Bergmann and contributors.


    Acceptance Tests (2) ---------
    ✔ LoginCest: Login successfully (0.02s)
    ✔ LoginCest: Login with invalid password (0.01s)
    ------------------------------

    Time: 273 ms, Memory: 11.50MB

    OK (2 tests, 0 assertions)
    ```

# Reference

1. Codeception Quickstart Guide
    * http://codeception.com/quickstart
1. Command lines
    * Count「phantomjs」processes running
        ```
        $ ps -ef | grep 'phantomjs' | grep -v grep | grep 4444 | wc -l
        1
        ```
    * Show「phantomjs」process
        ```
        $ ps -ef | grep 'phantomjs' | grep -v grep | grep 4444
        501 12708 12594   0  3:03PM ttys002    0:09.17 phantomjs --webdriver=4444 --ignore-ssl-errors=true --ssl-protocol=tlsv1
        ```
    * Kill「phantomjs」process by process id, for example:「12708」as above one.
        ```
        $ kill -9 12708
        ```
1. Codeception running in debug modes
    * `$ ./vendor/bin/codecept run --debug`
    * `$ ./vendor/bin/codecept run --steps`
        ```
        Codeception PHP Testing Framework v2.3.6
        Powered by PHPUnit 5.7.22 by Sebastian Bergmann and contributors.


        Acceptance Tests (2) ---------
        LoginCest: Login successfully
        Signature: LoginCest:loginSuccessfully
        Test: tests/LoginCest.php:loginSuccessfully
        Scenario --
        I am on page "/"
        PASSED


        LoginCest: Login with invalid password
        Signature: LoginCest:loginWithInvalidPassword
        Test: tests/LoginCest.php:loginWithInvalidPassword
        Scenario --
        I am on page "/"
        PASSED
        ------------------------------

        Time: 374 ms, Memory: 11.50MB

        OK (2 tests, 0 assertions)
        ```
1. Steps for testing with other browsers (firefox, ie, chrome, etc)
    * Download drivers, example: for chrome
        * https://sites.google.com/a/chromium.org/chromedriver/
        * Move to `/usr/local/bin/chromedriver`
    * [Download](http://www.seleniumhq.org/download/) and start「selenium-server-standalone」with browser driver
        * `java -Dwebdriver.chrome.driver="/opt/chromium-browser/chromedriver" -jar /usr/local/bin/selenium-server-standalone-3.4.0.jar`
    * Read more
        * https://github.com/lmc-eu/steward/wiki/Selenium-server-&-browser-drivers
