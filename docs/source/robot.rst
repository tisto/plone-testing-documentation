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


Rich Text
---------

todo


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
