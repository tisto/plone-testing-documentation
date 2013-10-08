Viewlets
========

Viewlet integration test

import unittest2 as unittest

from zope.component import queryMultiAdapter
from zope.viewlet.interfaces import IViewletManager

from Products.Five.browser import BrowserView as View

from jungzeelandia.theme.testing import MYCOMPANY_THEME_INTEGRATION_TESTING


class TestViewletsIntegration(unittest.TestCase):

    layer = MYCOMPANY_THEME_INTEGRATION_TESTING

    def setUp(self):
        self.portal = self.layer['portal']
        self.request = self.layer['request']
        from jungzeelandia.theme.interfaces import IMyCompanyTheme
        from zope.interface import alsoProvides
        alsoProvides(self.request, IMyCompanyTheme)

    def test_topnav_viewlet_is_present(self):
        view = View(self.portal, self.request)
        manager = queryMultiAdapter(
            (self.portal, self.request, view),
            IViewletManager,
            'plone.portalheader',
            default=None)
        self.failUnless(manager)
        manager.update()
        topnav_viewlet = [
            v for v in manager.viewlets \
            if v.__name__ == 'jungzeelandia.topnav']
        self.failUnlessEqual(len(topnav_viewlet), 1)

