zope.testbrowser
================

Debugging
---------

  open('/tmp/testbrowser.html', 'w').write(self.browser.contents)
  import pdb; pdb.set_trace()

Browser Login
-------------

  from plone.app.testing import TEST_USER_NAME
  from plone.app.testing import TEST_USER_PASSWORD
  self.browser.open(portal_url + '/login_form')
  self.browser.getControl(name='__ac_name').value = TEST_USER_NAME
  self.browser.getControl(name='__ac_password').value = TEST_USER_PASSWORD
  self.browser.getControl(name='submit').click()

Mock Mailhost
-------------

 from Products.CMFPlone.tests.utils import MockMailHost
 from Products.MailHost.interfaces import IMailHost
 from zope.component import getSiteManager
 portal._original_MailHost = portal.MailHost
 portal.MailHost = mailhost = MockMailHost('MailHost')
 sm = getSiteManager(context=portal)
 sm.unregisterUtility(provided=IMailHost)
 True
 sm.registerUtility(mailhost, provided=IMailHost)
 portal.email_from_address = "portal@plone.test"
 mailhost = portal.MailHost

Input
-----

todo

Text Area
---------

HTML::

  <textarea name="form.widgets.mytext"></textarea>

Test::

  self.browser.getControl(name='form.widgets.mytext').value = '<p>Lorem Ipsum</p>'

Radio Buttons
-------------

  self.browser.getControl(name='form.widgets.city:list').value = ['Berlin']

Checkboxes
----------

HTML::

  <input type="checkbox"
         value="selected"
         checked="checked"
         name="form.widgets.city:list">

Test::

  self.browser.getControl(
      name="form.widgets.city:list"
  ).value = ['checked']


Select
------

HTML::

 ...

Test::

  self.browser.getControl(
    'Site language'
  ).value = ['de']


Links
-----

  self.browser.getLink('Publish').click()


Buttons
-------

  self.browser.getControl('Save').click()

Image Upload
------------

  self.browser.getLink('Image').click()
  self.browser.getControl(name='form.widgets.title')\
    .value = "My image"
  self.browser.getControl(name='form.widgets.description')\
    .value = "This is my image."
  image_path = os.path.join(os.path.dirname(__file__), "image.png")
  image_ctl = self.browser.getControl(name='form.widgets.image')
  image_ctl.add_file(open(image_path), 'image/png', 'image.png')
  self.browser.getControl('Save').click()

File Upload
-----------

todo
