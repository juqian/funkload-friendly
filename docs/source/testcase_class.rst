=======================
Friendly TestCase Class
=======================

funkload-friendly provides a TestCase, it is extended FunkLoadTestCase.

This class features: 

- Retain cookie data between multiple requrests.
- Be able to configure log and result output directory, and some shortcut attribute.
- decorator for test description

Cookie Support
==============

In FunkLoad, cookie data doesn't retain between request.

In funkload-friendly, cookie data retain between request.
This means it easy to be able to test using the Cookie-based session.

**Example**:

.. code-block:: python

   class LoginTest(TestCase):
       def test_login(self):
           self.get(self.site_url + "/login/")  # take the CSRF token into cookie
           # Login with cookie (CSRF token required)
           self.post(self.site_url + "/login/",
               params=[
                   ['username', 'spam'],
                   ['password', 'P@ssw0rd'],
                   ['csrfmiddlewaretoken', self.cookie['csrftoken']],
               ]
           )

Configration Support
====================

funkload-friendly can be a configuration shown below.

Omit Filename
-------------

In FunkLoad, output destination of log and result is, you must specify the file name.

In funkload-friendly, specify a directory name, you can omit the file name.

**Example**:

.. code-block:: ini

   [ftest]
   log_to = console file
   log_directory = ftest/
   result_directory = ftest/

   [bench]
   log_to = console file
   log_directory = bench/
   result_directory = bench/

Shortcut Attribute
------------------

Some config value assign to the attribute of TestCase.

- section: ``[main]``, key: ``url`` map to ``TestCase.site_url``.

  Specify the ``url`` in ``http[s]://host[:port]`` format.

**Example**:

.. code-block:: ini

   [main]
   url = http://localhost:8000

Description Decorator
=====================

In FunkLoad, method description has must be described in config file.

In funkload-friendly, description can be described with the description descorator.

**Example**:

.. code-block:: python

   from funkload_friendly.test import TestCase, description


   class MainTest(TestCase):
       @description("Load top_page")
       def test_top_page(self):
           self.get(self.site_url + "/")

**Result(log)**::

   test_top_page: Starting -----------------------------------
           Load top_page
   test_top_page: GET: http://localhost:8000/
           Page 1:  ...
   test_top_page:  Done in 0.005s
   test_top_page:  Load css and images...
   test_top_page:   Done in 0.000s
