z3c.form
========

    def test_flag_article_as_inappropriate_with_comment(self):
        provideAdapter(adapts=(Interface, IBrowserRequest),
                       provides=Interface,
                       factory=FlagArticleAsInapproriateForm,
                       name=u"flag_as_inappropriate")
        flagAsInappropriateForm = getMultiAdapter(
            (self.article, self.request),
            name=u"flag_as_inappropriate")
        flagAsInappropriateForm.request.form = {
            'form.widgets.reason': 'This is a very naughty comment!'}
        flagAsInappropriateForm.update()
        data, errors = flagAsInappropriateForm\
            .extractData()

        self.assertEqual(len(errors), 0)
        flagAsInappropriateForm.handleApply(flagAsInappropriateForm, "foo")
        self.assertEquals('flagged',
                          self.wftool.getInfoFor(self.article, 'review_state'))
        self.assertEquals('This is a very naughty comment!',
                          self.wftool.getInfoFor(self.article, 'comments'))
        self.assertEquals(len(self.mailhost.messages), 1)

OLD WAY

from zope.interface import Interface
from zope.component import getMultiAdapter
from zope.component import createObject, queryUtility
from zope.component import provideAdapter
from zope.publisher.interfaces.browser import IBrowserRequest
from zope import interface
from zope.interface import alsoProvides
from zope.publisher.browser import TestRequest
from zope.annotation.interfaces import IAttributeAnnotatable
from z3c.form.interfaces import IFormLayer
from zope.component import getMultiAdapter
from freitag.membership.views.memberprofile import MemberProfileEditForm

...

    def test_edit_form(self):

        def make_request(form={}):
            request = TestRequest()
            request.form.update(form)
            alsoProvides(request, IFormLayer)
            alsoProvides(request, IAttributeAnnotatable)
            return request

        provideAdapter(adapts=(Interface, IBrowserRequest),
                       provides=Interface,
                       factory=MemberProfileEditForm,
                       name=u"personal-preferences")
        request = make_request(form={})
        editForm = getMultiAdapter((self.portal, request),
                                      name=u"personal-preferences")

        editForm.update()
        data, errors = editForm.extractData()
        self.assertEqual(len(errors), 0)


