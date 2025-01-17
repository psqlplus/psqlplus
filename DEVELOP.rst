Development Guide
-----------------
This is a guide for developers who would like to contribute to this project.

GitHub Workflow
---------------

If you're interested in contributing to psqlplus, first of all my heart felt
thanks. `Fork the project <https://github.com/psqlplus/psqlplus>`_ on github.  Then
clone your fork into your computer (``git clone <url-for-your-fork>``).  Make
the changes and create the commits in your local machine. Then push those
changes to your fork. Then click on the pull request icon on github and create
a new pull request. Add a description about the change and send it along. I
promise to review the pull request in a reasonable window of time and get back
to you.

In order to keep your fork up to date with any changes from mainline, add a new
git remote to your local copy called 'upstream' and point it to the main psqlplus
repo.

::

   $ git remote add upstream git@github.com:psqlplus/psqlplus.git

Once the 'upstream' end point is added you can then periodically do a ``git
pull upstream master`` to update your local copy and then do a ``git push
origin master`` to keep your own fork up to date.

Check Github's `Understanding the GitHub flow guide
<https://guides.github.com/introduction/flow/>`_ for a more detailed
explanation of this process.

Local Setup
-----------

The installation instructions in the README file are intended for users of
psqlplus. If you're developing psqlplus, you'll need to install it in a slightly
different way so you can see the effects of your changes right away without
having to go through the install cycle every time you change the code.

It is highly recommended to use virtualenv for development. If you don't know
what a virtualenv is, `this guide <http://docs.python-guide.org/en/latest/dev/virtualenvs/#virtual-environments>`_
will help you get started.

Create a virtualenv (let's call it psqlplus-dev). Activate it:

::

    source ./psqlplus-dev/bin/activate

    or

    .\psqlplus-dev\scripts\activate (for Windows)

Once the virtualenv is activated, `cd` into the local clone of psqlplus folder
and install psqlplus using pip as follows:

::

    $ pip install --editable .

    or

    $ pip install -e .

This will install the necessary dependencies as well as install psqlplus from the
working folder into the virtualenv. By installing it using `pip install -e`
we've linked the psqlplus installation with the working copy. Any changes made
to the code are immediately available in the installed version of psqlplus. This
makes it easy to change something in the code, launch psqlplus and check the
effects of your changes.

Adding PostgreSQL Special (Meta) Commands
-----------------------------------------

If you want to work on adding new meta-commands (such as `\dp`, `\ds`, `dy`),
you need to contribute to `pgspecial <https://github.com/psqlplus/pgspecial/>`_
project.

Visual Studio Code Debugging
-----------------------------
To set up Visual Studio Code to debug psqlplus requires a launch.json file.

Within the project, create a file: .vscode\\launch.json like below.

::

    {
        // Use IntelliSense to learn about possible attributes.
        // Hover to view descriptions of existing attributes.
        // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
        "version": "0.2.0",
        "configurations": [
            {
                "name": "Python: Module",
                "type": "python",
                "request": "launch",
                "module": "psqlplus.main",
                "justMyCode": false,
                "console": "externalTerminal",
                "env": {
                    "PGUSER": "postgres",
                    "PGPASS": "password",
                    "PGHOST": "localhost",
                    "PGPORT": "5432"
                }
            }
        ]
    }

Building RPM and DEB packages
-----------------------------

You will need Vagrant 1.7.2 or higher. In the project root there is a
Vagrantfile that is setup to do multi-vm provisioning. If you're setting things
up for the first time, then do:

::

    $ version=x.y.z vagrant up debian
    $ version=x.y.z vagrant up centos

If you already have those VMs setup and you're merely creating a new version of
DEB or RPM package, then you can do:

::

    $ version=x.y.z vagrant provision

That will create a .deb file and a .rpm file.

The deb package can be installed as follows:

::

    $ sudo dpkg -i psqlplus*.deb   # if dependencies are available.

    or

    $ sudo apt-get install -f psqlplus*.deb  # if dependencies are not available.


The rpm package can be installed as follows:

::

    $ sudo yum install psqlplus*.rpm

Running the integration tests
-----------------------------

Integration tests use `behave package <https://behave.readthedocs.io/>`_ and
pytest.
Configuration settings for this package are provided via a ``behave.ini`` file
in the ``tests`` directory.  An example::

    [behave]
    stderr_capture = false

    [behave.userdata]
    pg_test_user = dbuser
    pg_test_host = db.example.com
    pg_test_port = 30000

First, install the requirements for testing:

::
    $ pip install -U pip setuptools 
    $ pip install --no-cache-dir ".[sshtunnel]" 
    $ pip install -r requirements-dev.txt 

Ensure that the database user has permissions to create and drop test databases
by checking your ``pg_hba.conf`` file. The default user should be ``postgres``
at ``localhost``. Make sure the authentication method is set to ``trust``. If
you made any changes to your ``pg_hba.conf`` make sure to restart the postgres
service for the changes to take effect.

::

    # ONLY IF YOU MADE CHANGES TO YOUR pg_hba.conf FILE
    $ sudo service postgresql restart

After that, tests in the ``/psqlplus/tests`` directory can be run with:
(Note that these ``behave`` tests do not currently work when developing on Windows due to pexpect incompatibility.)

::

    # on directory /psqlplus/tests
    $ behave

And on the ``/psqlplus`` directory:

::

    # on directory /psqlplus
    $ py.test

To see stdout/stderr, use the following command:

::

    $ behave --no-capture

Troubleshooting the integration tests
-------------------------------------

- Make sure postgres instance on localhost is running
- Check your ``pg_hba.conf`` file to verify local connections are enabled
- Check `this issue <https://github.com/psqlplus/psqlplus/issues/945>`_ for relevant information.
- `File an issue <https://github.com/psqlplus/psqlplus/issues/new>`_.

Coding Style
------------

``psqlplus`` uses `black <https://github.com/ambv/black>`_ to format the source code. Make sure to install black.

Releases
--------

If you're the person responsible for releasing `psqlplus`, `this guide <https://github.com/psqlplus/psqlplus/blob/main/RELEASES.md>`_ is for you.
