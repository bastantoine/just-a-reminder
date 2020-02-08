Creating docs using Sphinx and readthedocs.org
==============================================

Creating the doc
----------------
First you need to install ``sphinx`` using ``pip`` (assuming you already have Python installed)::

    $ pip install sphinx

Sphinx comes with a script that helps you start a new project::

    $ sphinx-quickstart

You'll be asked for a few things:

- If you want to separate the build and source files (default to no)
- The project name (cannot be blank)
- The author(s) name(s) (cannot be blank either)
- The project release (press enter to live it blank)
- The project language (default to English (en), see `here <https://www.sphinx-doc.org/en/master/usage/configuration.html#confval-language>`_ for the supported codes)

This creates a few files, like ``index.rst`` which is the default entry point of you documentation, ``conf.py`` which handles the configuration of your project, a ``Makefile`` to, among other things, build the doc, and ``make.bat`` it's equivalent for Windows.

To build the documentation, run ``make html`` in the root folder (where the ``Makefile`` is) and open ``index.html`` inside the ``build`` folder.

Host it to readthedocs.org
------------------------------

`readthedocs.org <https://readthedocs.org>`_ (RTD) provides full and free hosting for various types of documentation (sphynx, Mkdocs,...).

To do so, you'll have to host your source code in a publically available server using a version control system (Git, SVN, Mercurial,...).

We'll be using a GitHub repo for the example, if you are using an other tool you'll have to adapt the commands.

Create the repository and start the project
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

First we start by creating a folder::

    $ mkdir documentation
    $ cd documentation

In this folder, we init a new git repository::

    $ git init

We can now start a new sphinx project in a ``docs`` folder using ``sphinx-quickstart`` (see above)::

    $ mkdir docs
    $ cd docs
    $ sphinx-quickstart

It's recommended to add a ``.gitignore`` file to avoid tracking the build files we have locally and some other files::

    # Builds
    # If you chose to have the source
    # and builds separated, use this:
    # docs/build/*
    # Otherwise use this:
    # docs/_build/*

    # make files are not required by RTD
    docs/make.bat
    docs/Makefile

We can now commit all the new files to instantiate the repository::

    $ git commit -a -m 'Init the repo'

We still need to add the remote to the GitHub repository and push the commit::

    $ git remote add origin https://github.com/your_name/your_repo
    $ git push -u origin master

We now have a fully fonctionnal sphinx project that can be hosted in RTD.

Start the project in RTD
^^^^^^^^^^^^^^^^^^^^^^^^

Start by creating an account on the plateform. You can either create yourself the account, or use one from GitHub, GitLab or Bitbucket. The later option will allow you to easily link a repo from the plateform to the RTD project.

Once this is done and you are connected, you can start a new project by clicking on the button *Import a new project*.

If you have linked you account to one of GitHub, GitLab or Bitbucket, you'll see the list of your repository on the plateform (you may have to refresh the list). In this case, you juste have to select the one you want to link.

Otherwise you have to manually link the project to you repo. To do so, click on *Import manually*. You'll be asked to enter a name, the link to your repo and the type of repo (Git, Subversion, Mercurial or Bazaar).

Once this is done, RTD might automatically start a build. Otherwise click on *Compile a version* to start the build.
