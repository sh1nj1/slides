layout: true
class: center, middle
---
# Tests With Selenium and RobotFramework

.footnote[
  by [Soonoh+](http://google.com/+SoonohJung)
]
---
layout: false
class: center, middle
# Issues

---
## Everyone has their own documents

- Product Manager - Specs
- Project Manager - Planning matrix. Meeting notes
- QA - Test cases, Test reports
- Developer - Issues on issue manager

---
class: center, middle
.key-idea[Define Single Source of Truth]

---
## Blocking others

- QA - The product is not ready to test.
- Developer - The spec is not enough to implement.

---
class: center, middle
.key-idea[Work concurrently and collaboratively]

---
## Not Automated, not repetitive

- Developer - How can I reproduce?
- QA - Blah, blah, ... well... I am not sure.

---
class: center, middle
.key-idea[Automate Tests, Reproduce every time]

---
class: center, middle
# Prepare Demo App

---
## Installation

This [Demo App](https://bitbucket.org/robotframework/webdemo/wiki/Home) provided by RobotFramework as an example.

For easy installation, use [Chocolatey](http://chocolatey.org) in Windows.
Install Python 2.7 or higher (Tested 2.7 in Windows 7)

```
@powershell -NoProfile -ExecutionPolicy unrestricted
-Command "iex ((new-object net.webclient)
.DownloadString('https://chocolatey.org/install.ps1'))"
&& SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
cinst msysgit
cinst python
cinst easy.install
cinst pip
```
Clone demo app from git repository.
```
$ git clone https://bitbucket.org/robotframework/webdemo.git
```
---
# Demo App
```
$ cd webdemo
$ python demoapp/server.py
Demo server starting on port 7272.
```
Open url http://localhost:7272 in Firefox browser.
It has a simple login screen. A valid ID and Password are "demo" and "mode".

![DemoApp-Login](https://bitbucket.org/robotframework/webdemo/wiki/demoapp.png)

We will continue with this demo app later.

---
class: center, middle
# Automate Web Test With Selenium IDE

---
## Selenium

Browser automation framework.

- Selenium IDE
- Selenium WebDriver (java, python, ruby, ...)
- Selenium Server
---
## Selenium IDE

Selenium IDE is a Firefox browser plugin.

- Record & Run Tests
- Auto Locator, Command Manual

There is a very good [User Manual](http://docs.seleniumhq.org/docs/02_selenium_ide.jsp)

---
## Installing

- open following URL in Firefox browser
  http://release.seleniumhq.org/selenium-ide/2.8.0/selenium-ide-2.8.0.xpi
- Or find and click the latest version in download screen
  http://www.seleniumhq.org/download/
- After installing, click Selenium IDE icon on the right top corner of the Firefox browser.
- Check out detailed installation process:
  http://www.askqtp.com/2013/02/how-to-install-selenium-ide-in-firefox.html

---
## Building Test Cases

Let's go back to our demo app.

[Watch demo video](http://www.youtube.com/watch?v=oIkjFEKnlPs) how to make and run tests.

[![Selenium IDE Demo](http://img.youtube.com/vi/oIkjFEKnlPs/0.jpg)](http://www.youtube.com/watch?v=oIkjFEKnlPs)

---

Valid_Login_Test

|  Command           | Target            | Value                                            |
|--------------------|-------------------|--------------------------------------------------|
| open               | /                 |                                                  |
| type               | id=username_field | demo                                             |
| type               | id=password_field | mode                                             |
| clickAndWait       | id=login_button   |                                                  |
| verifyTitle        | Welcome Page      |                                                  |
| verifyText         | css=p             | Login succeeded. Now you can logout.             |
| verifyText         | link=logout       | logout                                           |
| clickAndWait       | link=logout       | &nbsp;                                           |

Invalid_Login_Test

|  Command           | Target            | Value                                            |
|--------------------|-------------------|--------------------------------------------------|
| open               | /                 |                                                  |
| type               | id=username_field | invalid                                          |
| type               | id=password_field | invalid                                          |
| clickAndWait       | id=login_button   |                                                  |
| verifyTitle        | Error Page        |                                                  |
| verifyText         | css=p             | Login failed. Invalid user name and/or password. |

---
## Selenese

Selenese is the set of Selenium commands.
### Commands
- Actions
  - `clickAndWait`
  - `click`
- Accessors
  - `storeTextPresent`
  - `storeElementPresent`
- Assertions
  - `verifyTitle`
  - `assertText`

[Selenese Command Reference](http://release.seleniumhq.org/selenium-core/1.0.1/reference.html)

---
### Syntax

Selenium Tests from Selenium-IDE will be stored in an HTML text file format.
This consists of a HTML table with three columns.

Command, Target and Value (Actually latter two are same level of arguments of the command)

Target is usually a Locator locating an Element in HTML.
If it is `@id` or `@name` attribute it can be simply used as its value.

```html
<input type="submit" name="submit_button" />
```

| Command | Target        | Value  |
|---------|---------------|--------|
| click   | submit_button | &nbsp; |

---
### Locator

.left-column[
- Identifier
  ```
  identifier=loginForm
  loginForm
  ```
- Id
  ```
  id=loginForm
  ```
- Name
  ```
  name=username
  ```
- XPath
  ```
  xpath=/html/bodoy/form[1]
  //input[@name='username']
  ```
]
.right-column[
- Link Text
  ```
  link=Continue
  ```
- DOM
  ```
  dom=document.getElementById('loginForm')
  ```
- CSS
  ```
  css=form#loginForm
  ```
]
---
class: center, middle
# Writes Acceptance Tests With Robot Framework

---
## Robot Framework

Robot Framework is a generic open source test automation framework for acceptance testing and acceptance test-driven development (ATDD). It has easy-to-use tabular test data syntax and it utilizes the keyword-driven testing approach.

---
## Why Robot Framework

- A generic test framework
- Open sourced by Nokia in 2008
- Keyword driven and Easy to understand
- Extensible via Python and Java
- SBE
- Logging, Screen shots, Reports, Tagging
- Selenium2Labrary

---
# Install

Install RobotFramework and Selenium2 Library.

```
pip install robotframework
pip install robotframework-selenium2library
```

---
## Demo
To run RobotFramework tests, run following command in demp app directory.
```
pybot login_tests/
```
[Watch Demo Video](http://www.youtube.com/watch?v=-_Qw66HDRFs)

[![RobotFramework Demo](http://img.youtube.com/vi/-_Qw66HDRFs/0.jpg)](http://www.youtube.com/watch?v=-_Qw66HDRFs)

---
## Test Case

```ruby
**** Settings ***
Documentation     A test suite with a single test for valid login.
...
...               This test has a workflow that is created using keywords in
...               the imported resource file.
Resource          resource.txt

**** Test Cases ***
Valid Login
    Open Browser To Login Page
    Input Username    demo
    Input Password    mode
    Submit Credentials
    Welcome Page Should Be Open
    [Teardown]    Close Browser
```
---
## Resource

```ruby
**** Settings ***
Documentation     A resource file with reusable keywords and variables.
...
...               The system specific keywords created here form our own
...               domain specific language. They utilize keywords provided
...               by the imported Selenium2Library.
Library           Selenium2Library

**** Variables ***
${SERVER}         localhost:7272
${BROWSER}        Firefox
${DELAY}          0
${VALID USER}     demo
${VALID PASSWORD}    mode
${LOGIN URL}      http://${SERVER}/
${WELCOME URL}    http://${SERVER}/welcome.html
${ERROR URL}      http://${SERVER}/error.html
```
---
```ruby
**** Keywords ***
Open Browser To Login Page
    Open Browser    ${LOGIN URL}    ${BROWSER}
    Maximize Browser Window
    Set Selenium Speed    ${DELAY}
    Login Page Should Be Open

Login Page Should Be Open
    Title Should Be    Login Page

Go To Login Page
    Go To    ${LOGIN URL}
    Login Page Should Be Open
```
---
```ruby
Input Username
    [Arguments]    ${username}
    Input Text    username_field    ${username}

Input Password
    [Arguments]    ${password}
    Input Text    password_field    ${password}

Submit Credentials
    Click Button    login_button

Welcome Page Should Be Open
    Location Should Be    ${WELCOME URL}
    Title Should Be    Welcome Page
```

[Selenium2Library Keywords References](http://rtomac.github.io/robotframework-selenium2library/doc/Selenium2Library.html#Wait%20Until%20Page%20Contains
)
---
class: center, middle
# Implementation in Small Team

Just Put Two Cents In

---
## Specification by example

* Collaborative Approach
* Using Realistic Examples
* Creating a single source of truth

---
## Roles

- Product Manager

  Writes specs that is easy to write tests (Some form of similar to Robot tests but not complete test form).

- Software Tester

  Writes test cases repetitively using Selenium IDE. QA Engineer or Developer do not need to ask how to reproduce.

- QA Engineer

  Convert selenium tests to Robot Framework tests. Complete spec as Acceptance tests with Robot framework.

- Developer

  Set good element ID in web elements for readable tests. Implement some custom test library.

---
## Concurrent engineering

.key-idea[Spec - Examples - Test Case

(Single Source of Truth)]

.key-idea[Development - CI - QA

]

---
## Practices

- Keep track test files with VCS.
  ```
  acceptance-test
      src/test/java     - Custom Test Libraries
          test/selenese - Selenium IDE tests(not acutal tests)
          test/robot    - Acceptance Tests
```
- Share tests to all staffs. Everyone can reproduce exactly.
- Run Acceptance Test at every night.
- Development progress = How many tests passed.

---
class: center, middle

<!--
background-image: url(http://upload.wikimedia.org/wikipedia/commons/thumb/2/20/Cakeisalie.svg/240px-Cakeisalie.svg.png)
-->
# A Cake
.cake-is-a-lie[is a lie.]

---
## PhantomJS

PhantomJS is a headless WebKit.

You can run tests with PhantomJS without GUI environment.

Install [PhantomJS](http://phantomjs.org/) and set browser as "phantomjs" in "Open Browser" keyword's 2nd argument.

- Ubuntu -
  `sudo apt-get install phantomjs`
- Mac -
  `brew install phantomjs`

```
Open Browser ${url} phantomjs
```
You can run demo with PhantomJS without changing test resource file.
```
pybot --variable BROWSER:phantomjs login_tests
```

---
# References

- https://w3c.github.io/webdriver/webdriver-spec.html
- http://docs.seleniumhq.org/docs/02_selenium_ide.jsp
- http://robotframework.org/
- https://github.com/robotframework/robotframework
- http://files.meetup.com/2719372/SJ-SELENIUM-MEETUP_20130325.pdf
- http://specificationbyexample.com/key_ideas.html
- http://en.wikipedia.org/wiki/Specification_by_example
- http://en.wikipedia.org/wiki/Don't_repeat_yourself
- http://software-testing-tutorials-automation.blogspot.com/
- http://phantomjs.org/


