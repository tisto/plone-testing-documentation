Views
=====

Test view registration
----------------------

    def test_delete_view_registered(self):
        try:
            getMultiAdapter(
                (self.portal.mi.se.tc, self.request),
                name="delete"
            )
        except:
            self.fail("Delete view is not registered properly.")

Test with getMultiAdapter
-------------------------

from zope.component import getMultiAdapter

def test_view_is_registered(self)
    # Get view
    view = getMultiAdapter((self.portal, self.portal.REQUEST), name="create-user")
    # Put view to acquisition chain
    view = view.__of__(self.portal)
    # Call view
    self.failUnless(view())


Test with restrictedTraverse
----------------------------

def test_view_is_registered(self):
    view = self.portal.restrictedTraverse('@@list-products')
    self.failUnless(view)
    self.assertEquals(view(), 'ListProductsView')

Test view with parameter
------------------------

    def test_autocomplete_tags_view_registered(self):
        self.request.set('term', 'foo')
        view = getMultiAdapter((self.portal, self.request),
                               name="autocomplete-tags")
        view = view.__of__(self.portal)
        self.failUnless(view())

Test with restrictedTraverse and parameter
------------------------------------------

    def test_view_with_restrictedTraverse_and_params(self):
        view = self.context.restrictedTraverse("comment-statistics-batch")
        view = view.__of__(self.context)
        view(query, base_number * i, base_number * (i + 1) - 1)

Test if view is protected
-------------------------

def test_view_is_protected(self):
    from AccessControl import Unauthorized
    self.logout()
    self.assertRaises(Unauthorized,
                      self.portal.restrictedTraverse,
                      '@@deploymentmanager')

Test if object exists in folder
-------------------------------

def test_object_in_folder(self):
    self.failIf('yoda' in self.portal.objectIds())

Test Redirect
-------------

    def test_component_view(self):
        self.portal.mi.sec.invokeFactory(
            "TextComponent",
            id="tx",
            title="Text Component 1",
        )
        view = getMultiAdapter(
            (self.portal.mi.sec.tx, self.request),
            name="view"
        )
        view = view.__of__(self.portal.mi.sec)

        view()

        self.assertEqual(
            self.request.response.headers['location'],
            'http://nohost/plone/mi/sec'
        )

Test View HTML Output
=====================

    from lxml import html
    output = lxml.html.fromstring(view())
    self.assertEqual(len(output.xpath("/html/body/div")), 1)


Troubleshooting
===============

KeyError: 'ACTUAL_URL'

    def setUp(self):
        self.portal = self.layer['portal']
        self.request = self.layer['request']
        setRoles(self.portal, TEST_USER_ID, ['Manager'])
        self.portal.invokeFactory('Folder', 'test-folder')
        self.folder = self.portal['test-folder']
        self.request.set('URL', self.folder.absolute_url())
        self.request.set('ACTUAL_URL', self.folder.absolute_url())


    def test_view(self):
        view = self.collection.restrictedTraverse('@@RSS')
        self.assertTrue(view())
        self.assertEquals(view.request.response.status, 200)


ComponentLookupError: ((<PloneSite at /plone>, <HTTPRequest, URL=http://nohost/plone>), <InterfaceClass zope.interface.Interface>, 'recipes')


    def setUp(self):
        self.portal = self.layer['portal']
        self.request = self.layer['request']
        self.request.set('URL', self.portal.absolute_url())
        self.request.set('ACTUAL_URL', self.portal.absolute_url())
        from zope.interface import directlyProvides
        directlyProvides(self.request, IJungzeelandiaContenttypes)
        setRoles(self.portal, TEST_USER_ID, ['Manager'])


Test View Methods
=================

def test_method_sections(self):
        self.portal.mi.invokeFactory("Section", id="s1", title="Section 1")
        self.portal.mi.invokeFactory("Section", id="s2", title="Section 2")
        view = getMultiAdapter(
            (self.portal.mi, self.request),
            name="view"
        )
        view = view.__of__(self.portal.mi)

        self.assertEqual(len(view.sections()), 2)
        self.assertEqual(
            [x.title for x in view.sections()]
            [u'Section 1', u'Section 2']
        )

View Status Messages

    def test_delete_comments_sets_status_message(self):
        view = getMultiAdapter(
            (self.portal.mi.se.tc, self.request),
            name="delete"
        )
        view.__of__(self.portal.mi.se)

        view()

        self.assertEqual(
            IStatusMessage(self.request).show()[0].message,
            u'Item deleted'
        )

View Class

class DeleteComponent(BrowserView):

    def __call__(self):
        section = aq_parent(self.context)
        section.manage_delObjects([self.context.id])
        IStatusMessage(self.context.REQUEST).addStatusMessage(
            _("Item deleted"),
            type="info"
        )
        self.request.response.redirect(section.absolute_url())

