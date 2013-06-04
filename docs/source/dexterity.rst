Dexterity
=========

class DepartmentIntegrationTest(unittest.TestCase):

    layer = FREITAG_OVERVIEW_INTEGRATION_TESTING

    def setUp(self):
        self.portal = self.layer['portal']
        self.request = self.layer['request']
        self.request['ACTUAL_URL'] = self.portal.absolute_url()
        setRoles(self.portal, TEST_USER_ID, ['Manager'])
        self.portal.invokeFactory('Folder', 'test-folder')
        self.folder = self.portal['test-folder']
        self.portal.invokeFactory('freitag.overview.overview', id="overview1")
        self.overview = self.portal.overview1

    def test_schema(self):
        fti = queryUtility(IDexterityFTI,
                           name='freitag.overview.department')
        schema = fti.lookupSchema()
        self.assertEquals(IDepartment, schema)

    def test_fti(self):
        fti = queryUtility(IDexterityFTI,
                           name='freitag.overview.department')
        self.assertNotEquals(None, fti)

    def test_factory(self):
        fti = queryUtility(IDexterityFTI,
                           name='freitag.overview.department')
        factory = fti.factory
        new_object = createObject(factory)
        self.failUnless(IDepartment.providedBy(new_object))

    def test_adding(self):
        self.overview.invokeFactory('freitag.overview.department',
                                    'department1')
        department1 = self.overview['department1']
        self.failUnless(IDepartment.providedBy(department1))

    def test_global_adding_disallowed(self):
        self.assertRaises(ValueError,
                      self.folder.invokeFactory,
                      'freitag.overview.department',
                      'department1')



Test Reference to file

    def test_method_render_pdf_file_component(self):
        # Create file to reference to
        self.portal.invokeFactory('File', 'pdf_file')
        pdf_file = os.path.join(
            os.path.dirname(__file__), 'content', u'loremipsum.pdf'
        )
        self.portal.pdf_file.file = NamedBlobFile(
            data=open(pdf_file, 'r').read(),
            contentType='application/pdf',
            filename=u'loremipsum.pdf'
        )
        from zope import component
        from zope.app.intid.interfaces import IIntIds
        intids = component.getUtility(IIntIds)
        pdf_file_id = intids.getId(self.portal.pdf_file)
        # Create file component
        self.portal.mi.eb.invokeFactory('FileComponent', 'fc')
        from z3c.relationfield import RelationValue
        self.portal.mi.eb.fc.file = RelationValue(pdf_file_id)
        self.assertTrue(self.portal.mi.eb.fc.render())
        self.assertEqual(
            self.portal.mi.eb.fc.render(),
            u'<a href="http://nohost/plone/pdf_file/@@download">'
            u'loremipsum.pdf</a>'
        )

Permissions

    def test_add_medical_information_permission(self):
        self.portal.invokeFactory('MedicalInformation', 'mi1')
        sm = getSecurityManager()
        self.assertTrue(
            sm.checkPermission("dkg.addMedicalInformation", self.portal.mi1)
        )

    def test_editor_has_add_permission(self):
        logout()
        setRoles(self.portal, TEST_USER_ID, ['Editor'])
        login(self.portal, TEST_USER_NAME)
        self.assertTrue(
            self.portal.invokeFactory('MedicalInformation', 'mi1'),
            "The 'Editor' role does not possess the " +
            "'dkg.addMedicalInformation' permission."
        )

    def test_external_typist_has_add_permission(self):
        logout()
        setRoles(self.portal, TEST_USER_ID, ['External Typist'])
        login(self.portal, TEST_USER_NAME)
        self.assertTrue(
            self.portal.invokeFactory('MedicalInformation', 'mi1'),
            "The 'External Typist' role does not possess the " +
            "'dkg.addMedicalInformation' permission."
        )

