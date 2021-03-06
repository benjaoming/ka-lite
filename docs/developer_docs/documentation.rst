How to contribute to documentation
==================================

You can propose changes to the docs directly on Github (instructions below) or
email your recommendations to info@learningequality.org.

To propose changes directly, you'll need to create an account on github and open
a pull request. This document assumes you are somewhat familiar with that
process, and will not explain all the steps in detail. For full instructions on
how to make a pull request, see
`Github's help section <https://help.github.com/articles/creating-a-pull-request/>`_.

Documentation development
-------------------------

#. Work from the *develop* branch.
#. From the base directory, the documentation can be found in the ``docs/``
   subdirectory. Specific pages of the docs are each associated with a different
   .rst file, potentially in a subdirectory of ``docs/``.
#. The documentation is written in
   `ReStructured Text <http://sphinx-doc.org/rest.html>`_ format, so please see
   the primer!
#. After making your changes, try to build the docs to review them. This process
   can take some time, as an instance of the server and a browser may need to
   be started. To build the docs::
   
       pip install -r requirements_sphinx.txt  # To install software for building docs
       make docs

#. You can view the docs in a browser by opening *docs/_build/html/index.html*.
#. After you are satisfied with your changes push them to your fork of the ka-lite project, and then open a PR.

Screenshots
-----------

Screenshots are made automatically following the screenshots directives. They are stored in a folder ``docs/_static/``, which is also sync’ed to Github.

To grab new screenshots, you have to have Firefox installed and run::

    cd docs/
    SPHINX_SS_USE_PVD=true make SPHINXOPTS="-D screenshots_create=1" html

Usage of screenshot Sphinx directive
____________________________________

Example code::

    .. screenshot::
      :navigation-steps: LOGIN admin superpassword
      :focus: #id_username | Enter your username and password using this form!
      :class: screenshot


.. warning:: Read the following, but then read :ref:`thing-about-click` before using the screenshot directive.

A screenshot directive has a number of required options:

 * ``:user-role:`` which can be one of ``guest``, ``learner``, ``coach``, or ``admin``, and determines what kind of user is logged in for the screenshot.
 * ``:url:`` which is the relative URL to take the screenshot at (or start a sequence of navigation actions to get to the page to take the screenshot at). Don't use urls with UUIDs, as those are dependent on the database and we can't guarantee what will be in the database for screenshot purposes.
 * ``:navigation-steps:`` which is a sequence of steps that needs to be taken.
   See the docstring of _parse_nav_steps in screenshot.py file for specification.
   If you find yourself writing the same sequence of nav steps over and over again, possibly with minor variations, then we can write an alias for it as for LOGIN.
 * ``:focus:`` which will highlight an element specified by jQuery-style selector (by giving it a nice red border) and optionally add a floating annotation box. Works like this:
    :focus: the_selector | A nice little message that follows.

Just use arbitrary selection a la jQuery (so put a hash in front of IDs, etc). Whitespace can be included, e.g. ``li a.classname``. Use the separator for annotations!


.. _thing-about-click:

The thing about ``click``
_________________________

The ``:navigation-steps:`` option is meant to simulate user interaction for screenshots. Unfortunately, it can be dangerous!

Basically, if you use ``click`` on an element and want to use the *result* of the click, you can get into trouble if the result takes a long time.

For example, something like ``#link_to_another_page click`` that causes a page load is probably a mistake -- the directive doesn't know how long to wait for the page to load, and will probably throw an error.

Using ``click`` to interact with UI elements *on the same page* should be ok, as long as the result of interacting with the UI element happens really, really fast.

The rule of thumb is, if a ``click`` loads a page or waits for an AJAX result, think twice about using it.

