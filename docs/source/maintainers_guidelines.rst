========================
Maintainers Guidelines
========================


The section is dedicated to the ximage core developers (maintainers).


List of the core contributors
=================================

* Current Release Manager : Ghiggi Gionata
* Testing Team : Ghiggi Gionata



Versions guidelines
========================

ximage uses `Semantic <https://semver.org/>`_ Versioning. Each release is associated with a git tag of the form X.Y.Z.

Given a version number in the MAJOR.MINOR.PATCH (eg., X.Y.Z) format, here are the differences in these terms:

- MAJOR version - make breaking/incompatible API changes
- MINOR version - add functionality in a backwards compatible manner. Adding new reader
- PATCH version - make backwards compatible bug fixes


Breaking vs. non-breaking changes
-----------------------------------

Since ximage is used by a broad ecosystem of both API consumers and implementers, it needs a strict definition of what changes are “non-breaking” and are therefore allowed in MINOR and PATCH releases.

In the ximage specifications, a breaking change is any change that requires either consumers or implementers to modify their code for it to continue to function correctly.

Examples of breaking changes include:

- Adding new functionalities to the ximage that affect the behavior of the software directly.
- Modify the argument name of an ximage function.

Examples of non-breaking changes include :

- Fix a bug.
- Adding new features
- Adding new functionalities to the ximage API that do not affect the behavior of the API directly.
- Updating the documentation.
- Internal function refactoring that does not affect the behavior of the software directly.

Ongoing version support
-----------------------------------

ximage major releases aims to move the community forward, focusing on specifications stabilization and major feature additions, rather than backwards-compatibility. ximage minor releases will be backwards compatible. We strongly recommend adopting the latest release of ximage into production within 6 months for major releases, and 4 months for minor releases.

The maintaners do their best but does not guarantee any period of support or maintenance.

Releases that are 2 years or older may be considered as deprecated.


Documentation
===============

ximage's documentation is built using the powerful `Sphinx <https://www.sphinx-doc.org/en/master/>`_ framework,
styled with `Book Theme <https://sphinx-book-theme.readthedocs.io/en/stable/index.html>`_.

All documentation source files are neatly organized in the ``docs/`` directory within the project's repository.


Documentation generation
--------------------------

To build the documentation locally, follow the next three steps.

1. Set up the python environment for building the documentation

	The python packages required to build the documentation are listed in the `environment.yaml <https://github.com/ghiggi/ximage/blob/main/docs/environment.yaml>`_ file.

	For an efficient setup, we recommend creating a dedicated virtual environment.
	Navigate to the ``docs/`` directory and execute the following command. This will create a new environment and install the required packages:

	.. code-block:: bash

		conda create -f environment.yaml

2. Activate the virtual environment

	Once the environment is ready, activate it using:

	.. code-block:: bash

    	conda activate build-doc-ximage

3. Generate the documentation

	With the environment set and activated, you're ready to generate the documentation.
	Execute:

	.. code-block:: bash

		make clean html

	This command will build the HTML version of the documentation.
	It first cleans previous builds (``make clean``) and then generates fresh documentation (``html``).

	.. note:: It's important to review the output of the command. Look out for warnings or errors and address them to ensure the documentation is accurate and complete.

By following these steps, you should have a local version of the ximage documentation
in the ``docs/build/html/`` directory, ready for review or deployment!


Documentation deployment
----------------------------

A webhook is defined in the GitHub repository to trigger automatically the publication process to
`ReadTheDocs <https://about.readthedocs.com/?ref=readthedocs.com>`__
after each Pull Request.

This webhook is linked to the ximage core developer.

.. image:: /static/documentation_release.png

Ghiggi Gionata owns the `ReadTheDoc <https://readthedocs.org/>`__ account.


Package release 
================== 

A `GitHub Action <https://github.com/ghiggi/ximage/actions>`_ is configured to automate the packaging and uploading process
to `PyPI <https://pypi.org/project/ximage/>`_.
This action, detailed `here <https://github.com/ghiggi/ximage/blob/main/.github/workflows/release_to_pypi.yml>`_,
triggers the packaging workflow depicted in the following image:

One  `GitHub Action <https://github.com/ghiggi/ximage/actions>`_ is defined to trigger the packaging and the upload on `pypi.org <https://pypi.org/project/ximage/>`_.

.. image:: /static/package_release.png


Upon the release of the package on PyPI, a conda-forge bot attempts to automatically update the
`conda-forge recipe <https://github.com/conda-forge/ximage-feedstock/>`__.
Once the conda-forge recipe is updated, a new conda-forge package is released.

The PyPI project and the conda-forge recipes are collaboratively maintained by core contributors of the project.


Release process
---------------

Before releasing a new version, the ``CHANGELOG.md`` file should be updated. Run

.. code-block:: bash

    make changelog X.Y.Z

to update the ``CHANGELOG.md`` file with the list of issues and pull requests that have been closed since the last release.
Manually add a description to the release if necessary.
Then, commit the new ``CHANGELOG.md`` file.

.. code-block:: bash

    git add CHANGELOG.md
    git commit -m "update CHANGELOG.md for version X.Y.Z"
    git push

Create a new tag to trigger the release process.

.. code-block:: bash

    git tag -a vX.Y.Z -m "Version X.Y.Z"
    git push --tags

On GitHub, edit the release description to add the list of changes from the ``CHANGELOG.md`` file.


Reviewing process
=====================


The main branch is protected and requires at least one review before merging.

The review process is the following:

#. A PR is opened by a contributor
#. The CI pipeline is triggered and the status of the tests is reported in the PR.
#. A core contributor reviews the PR and request changes if needed.
#. The contributor updates the PR according to the review.
#. The core contributor reviews the PR again and merge it if the changes are ok.



Continuous integration
========================

Continuous Integration (CI) is a crucial practice in modern software development, ensuring that code changes are regularly integrated into the main codebase.
With CI, each commit or pull request triggers an automated process that verifies the integrity of the codebase, runs tests,
and performs various checks to catch issues early in the development lifecycle.

The table below summarizes the software tools utilized in our CI pipeline, describes their respective aims and project pages.


+----------------------------------------------------------------------------------------------------+------------------------------------------------------------------+----------------------------------------------------------------------------------------------+
|  Tools                                                                                             | Aim                                                              | Project page                                                                                 |
+====================================================================================================+==================================================================+==============================================================================================+
| `Pytest  <https://docs.pytest.org>`__                                                              | Execute unit tests and functional tests                          |                                                                                              |
+----------------------------------------------------------------------------------------------------+------------------------------------------------------------------+----------------------------------------------------------------------------------------------+
| `Black <https://black.readthedocs.io/en/stable/>`__                                                | Python code formatter                                            |                                                                                              |
+----------------------------------------------------------------------------------------------------+------------------------------------------------------------------+----------------------------------------------------------------------------------------------+
| `Ruff  <https://github.com/charliermarsh/ruff>`__                                                  | Python linter                                                    |                                                                                              |
+----------------------------------------------------------------------------------------------------+------------------------------------------------------------------+----------------------------------------------------------------------------------------------+
| `pre-commit.ci   <https://pre-commit.ci/>`__                                                       | Run pre-commit as defined in `.pre-commit-config.yaml <https://github.com/ghiggi/ximage/blob/main/.pre-commit-config.yaml>`__                                   |
+----------------------------------------------------------------------------------------------------+------------------------------------------------------------------+----------------------------------------------------------------------------------------------+
| `Coverage   <https://coverage.readthedocs.io/>`__                                                  | Measure the code coverage of the project's unit tests            |                                                                                              |
+----------------------------------------------------------------------------------------------------+------------------------------------------------------------------+----------------------------------------------------------------------------------------------+
| `CodeCov    <https://about.codecov.io/>`__                                                         | Uses Coverage to track and analyze code coverage over time.      | `ximage  <https://app.codecov.io/gh/ghiggi/ximage>`__                                        |
+----------------------------------------------------------------------------------------------------+------------------------------------------------------------------+----------------------------------------------------------------------------------------------+
| `Coveralls    <https://coveralls.io/>`__                                                           | Uses Coverage to track and analyze code coverage over time.      | `ximage  <https://coveralls.io/github/ghiggi/ximage>`__                                      |
+----------------------------------------------------------------------------------------------------+------------------------------------------------------------------+----------------------------------------------------------------------------------------------+
| `CodeBeat      <https://codebeat.co/>`__                                                           | Automated code review and analysis tools                         | `ximage <https://codebeat.co/projects/github-com-ghiggi-ximage-main>`__                      |
+----------------------------------------------------------------------------------------------------+------------------------------------------------------------------+----------------------------------------------------------------------------------------------+
| `CodeScene <https://codescene.com/>`__                                                             | Automated code review and analysis tools                         | `ximage <https://codescene.io/projects/41869/>`__                                            |
+----------------------------------------------------------------------------------------------------+------------------------------------------------------------------+----------------------------------------------------------------------------------------------+
| `CodeFactor <https://www.codefactor.io/>`__                                                        | Automated code review and analysis tools                         | `ximage <https://www.codefactor.io/repository/github/ghiggi/ximage>`__                       |
+----------------------------------------------------------------------------------------------------+------------------------------------------------------------------+----------------------------------------------------------------------------------------------+
