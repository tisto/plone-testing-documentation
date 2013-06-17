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

You can now run only the latter test: ./bin/test -m der.freitag -t working_on_it (This is Plone-specific. See Asko's comment below.)


..note: http://keeshink.blogspot.de/2013/03/robot-framework-testing-hints.html
