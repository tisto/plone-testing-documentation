Robot Framework
===============

Text Field
----------

HTML::

  <input type="text" name="form.widgets.title">

Robot Selector::

  Input Text  name=form.widgets.title  My Title


Text Area
---------

todo


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
