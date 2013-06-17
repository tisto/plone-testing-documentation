MISC
====

Login

self.loginAsPortalMember()
self.loginAsPortalOwner()
self.logout()
self.login("myusername")

Generator

def test_generator(self):
    self.assertEquals(sum(1 for w in self.viewlet.get_replies(workflow_actions=True)), 2)
    replies = self.viewlet.get_replies(workflow_actions=True)
    replies.next()
    replies.next()
    self.assertRaises(StopIteration, replies.next)

Redirect

def test_is_redirected(self):
    # XXX: not working!!!
    self.assertEquals(200, self.app.REQUEST.response.getStatus())
