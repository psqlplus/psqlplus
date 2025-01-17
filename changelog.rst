Dev 5.0
=======

Features
--------
* Add compatability to also connect to Oracle(tm) database
* Add little compatbility features to make more friendly for Oracle SQL*Plus(tm) users
* Add a `--ping` command line option; allows to replace `pg_isready`
* Changed the packaging metadata from setup.py to pyproject.toml

Bug fixes:
----------
* Avoid raising `NameError` when exiting unsuccessfully in some cases


4.1.0 (2024-03-09)
==================

Features:
---------
* Support `PGAPPNAME` as an environment variable and `--application-name` as a command line argument.
* Add `verbose_errors` config and `\v` special command which enable the
  displaying of all Postgres error fields received.
* Show Postgres notifications.
* Support sqlparse 0.5.x
* Add `--log-file [filename]` cli argument and `\log-file [filename]` special commands to
  log to an external file in addition to the normal output

Bug fixes:
----------

* Fix display of "short host" in prompt (with `\h`) for IPv4 addresses ([issue 964](https://github.com/dbcli/pgcli/issues/964)).
* Fix backwards display of NOTICEs from a Function ([issue 1443](https://github.com/dbcli/pgcli/issues/1443))
* Fix psycopg errors when installing on Windows.  ([issue 1413](https://https://github.com/dbcli/pgcli/issues/1413))
* Use a home-made function to display query duration instead of relying on a third-party library (the general behaviour does not change), which fixes the installation of `pgcli` on 32-bit architectures ([issue 1451](https://github.com/dbcli/pgcli/issues/1451))

Internal Changes:
-----------------

