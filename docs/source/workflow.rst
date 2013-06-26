Workflows, Roles and Permissions
================================

Role installed
--------------

Test if a certain role ('External Editor') has been installed::

    def test_external_editor_role_installed(self):
        self.assertTrue('External Editor' in self.wftool.validRoles())

profiles/default/rolemap.xml::

    <?xml version="1.0"?>
    <rolemap>
      <roles>
        <role name="External Editor" />
      </roles>
    </rolemap>


Roles has permission
--------------------

Test if a certain role ('External Editor') has a certain permission
('request review')::

    def test_external_editor_has_request_review_permission(self):
        self.assertEqual(
            [
                x for x in self.portal.rolesOfPermission('Request review')
                if x['name'] == 'External Editor'
            ][0]['selected'],
            'SELECTED',
            'External Editor role does not possess the "Request review" '
            'permission')

Workflow installed
------------------

    def test_workflows_installed(self):

        """Make sure both comment workflows have been installed properly."""
        self.assertTrue(
            ‘one_state_workflow’ in self.portal.portal_workflow.objectIds())

        self.assertTrue(
            ‘comment_review_workflow’ in self.portal.portal_workflow.objectIds())


Default workflow
----------------

    def test_default_workflow(self):

        """Make sure one_state_workflow is the default workflow."""

        self.assertEqual(
            (‘one_state_workflow’,),
            self.portal.portal_workflow.getChainForPortalType(
                ‘Discussion Item’
            )
        )

