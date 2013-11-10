Robot Framework
===============

Text Field
----------

HTML::

  <input type="text" name="form.widgets.title">

Robot Selector::

  Input Text  name=form.widgets.title  My Title

..more: http://rtomac.github.io/robotframework-selenium2library/doc/Selenium2Library.html#Input%20Text


Text Area
---------

HTML::

  <textarea name="form.widgets.description"></textarea>

Robot Selector::

  Input Text  name=form.widgets.title  My Text

..more: http://rtomac.github.io/robotframework-selenium2library/doc/Selenium2Library.html#Input%20Text


Rich Text (TinyMCE)
-------------------

HTML::

  <textarea
    class="mce_editable"
    id="form.widgets.text"
    name="form.widgets.text"></textarea>
  <span id="form.widgets.text_parent" class="mceEditor">
    <table id="form.widgets.text_tbl" class="mceLayout">
      <tbody>
        <tr class="mceFirst">...</td>
        <tr>
          <td class="mceIframeContainer mceFirst mceLast">
            <iframe id="form.widgets.test_ifr">
              <html>
                <head>...</head>
                <body id="content">...</body>
              </html>
            </iframe>
        </tr>
        <tr class="mceLast">...</tr>
      </tbody>
    </table>
  </span>

Robot Selector::

  Select frame  id=form.widgets.text_ifr
  Input text  id=content  My Rich Text
  Unselect Frame

Robot Keyword::

  Input RichText
    [Arguments]  ${input}
    Select frame  id=form.widgets.text_ifr
    Input text  id=content  ${input}
    Unselect Frame


.. more:

    http://keeshink.blogspot.de/2013/03/robot-framework-testing-hints.html


Checkbox
--------

HTML::

  <input
    type="checkbox"
    value="Cologne"
    name="form.widgets.city:list">

Robot Selector::

  Select Checkbox  xpath=//input[@name='form.widgets.city:list' and @value='Cologne']

.. more:

  http://rtomac.github.io/robotframework-selenium2library/doc/Selenium2Library.html#Select%20Checkbox


Radiobox
--------

todo


Select
------

todo


File
----

Before we can upload a file, we have to put a dummy file somewhere on the file
system and make sure it can be looked up in the test. Because robot tests can not look up files in relative paths, we have to pass the buildout directory as a environment variable to the test. To do so we have to amend the
```buildout.cfg```::

  [environment]
  BUILDOUT_DIR = ${buildout:directory}

  [test]
  recipe = zc.recipe.testrunner
  ...
  environment = environment

In our robot test directory, we create a python file that reads the
BUILDOUT_DIR variable from the environment and creates a PATH_TO_TEST_FILES variable from this ```tests/robot/variables.py```::

  PATH_TO_TEST_FILES = os.environ.get('BUILDOUT_DIR', '') + \
      '/src/plone.app.contenttypes/plone/app/contenttypes/tests'

Now we can put a dummy file into our tests directory and look it up in our
tests.

In order to make the PATH_TO_TEST_FILES variable available we have to include
the variables.py variables in our "Settings" part of our robot test file
```tests/robot/test_example.robot```::

  *** Settings ***

  Variables  dkg/policy/tests/acceptance/variables.py

  ...

  *** Keywords ***

  ...

  a file '${title}'
    Go to  ${PLONE_URL}/++add++File
    Input Text  name=form.widgets.title  ${title}
    Choose File  name=form.widgets.file  ${PATH_TO_TEST_FILES}/loremipsum.pdf
    Click Button  Save
    Wait until page contains  Item created
    Page Should Contain  loremipsum.pdf


Image
-----

Set up PATH_TO_TEST_FILES variable as described in the file section.

tests/robot/test_example.robot::

  an image '${title}'
    Go to  ${PLONE_URL}/++add++Image
    Input Text  name=form.widgets.title  ${title}
    Choose File  name=form.widgets.image  ${PATH_TO_TEST_FILES}/logo.jpg
    Click Button  Save
    Wait until page contains  Item created


Tags
----

RF supports tags. Add a line [Tags] tag1 tag2:

*** Test cases ***

Scenario: Clicking the submit button hides it
  Given i am logged in
    and i am on an article
   When i simulate clicking the comment submit button
   Then the submit button has class disabled

Scenario: Submitting a comment displays it in the page
[Tags] working_on_it
  Given i am logged in
    and i am on an article
   When i type something in the comment box
    and i click the comment submit button
   Then the page shows the comment

You can now run only the latter test: ./bin/test -m mycompany.package -t working_on_it (This is Plone-specific. See Asko's comment below.)


..note: http://keeshink.blogspot.de/2013/03/robot-framework-testing-hints.html

XPATH
-----

count(//td[text()='&nbsp;'])

  <strong id="search-results-number">


  Wait until keyword succeeds  10s  1s  XPath Should Match X Times  //strong[@id='search-results-number' and contains(.,'1')]  ${result_count}

