Mock Attribute::

    >>> m = MagicMock()
    >>> p = PropertyMock(return_value=3)
    >>> type(m).foo = p
    >>> m.foo
    3
    >>> p.assert_called_once_with()

--

    mock_file = mock.MagicMock()
    type(mock_file).open = mock.MagicMock(return_value=None)
    type(mock_file).read = mock.MagicMock(return_value='''name;city;country\njohn;boston;usa''')
    type(mock_file).closed = mock.PropertyMock(return_value=False)
    assert mock_file.open() == None
    assert mock_file.read() == "name;city;country\njohn;boston;usa"
    assert mock_file.closed == False

--

import mock
import pytest
import StringIO

from django.core.files.uploadedfile import UploadedFile
from django.core.files.uploadedfile import SimpleUploadedFile


def test_csv_import():
    from pux.util.csv_import import get_csv_data
    file_mock = mock.MagicMock(spec=file, wraps=StringIO.StringIO('test'))
    pytest.set_trace()
    get_csv_data(file_mock)
