==================
Sphinx Integration
==================

cliff supports integration with Sphinx by way of a `Sphinx directives`__.

.. rst:directive:: .. autoprogram-cliff:: namespace

   Automatically document an instance of :py:class:`cliff.command.Command`,
   including a description, usage summary, and overview of all options.

   .. code-block:: rst

       .. autoprogram-cliff:: openstack.compute.v2
          :command: server add fixed ip

   One argument is required, corresponding to the namespace that the command(s)
   can be found in. This is generally defined in the `entry_points` section of
   either `setup.cfg` or `setup.py`. Refer to the example_ below for more
   information.

   In addition, the following directive options can be supplied:

   `:command:`

     The name of the command, as it would appear if called from the command
     line without the executable name. This will be defined in `setup.cfg` or
     `setup.py` albeit with underscores. This is optional and `fnmatch-style`__
     wildcarding is supported. Refer to the example_ below for more
     information.

     .. seealso:: The ``autoprogram_cliff_application`` configuration option.

   `:ignored:`

     A comma-separated list of options to exclude from documentation for this
     option. This is useful for options that are of low value.

     .. seealso:: The ``autoprogram_cliff_ignored`` configuration option.

   The following global configuration values are supported. These should be
   placed in `conf.py`:

   `autoprogram_cliff_application`

     The top-level application name, which will be prefixed before all
     commands.  This is generally defined in the `console_scripts` attribute of
     the `entry_points` section of either `setup.cfg` or `setup.py`. Refer to
     the example_ below for more information.

     For example:

     .. code-block:: python

        autoprogram_cliff_application = 'my-sample-application'

     Defaults to ``''``

     .. seealso:: The ``:command:`` directive option.

   `autoprogram_cliff_ignored`

     A global list of options to exclude from documentation. This can be used
     to prevent duplication of common options, such as those used for
     pagination, across **all** options.

     For example:

     .. code-block:: python

        autoprogram_cliff_ignored = ['--help', '--page', '--order']

     Defaults to ``['--help']``

     .. seealso:: The ``:ignored:`` directive option.

.. seealso::

    Module `sphinxcontrib.autoprogram`
      An equivalent library for use with plain-old `argparse` applications.

    Module `sphinx-click`
      An equivalent library for use with `click` applications.

.. important::

    The :rst:dir:`autoprogram-cliff` directive emits :rst:dir:`code-block`
    snippets marked up as `shell` code. This requires `pygments` >= 0.6.

.. _example:

Example
=======

Take a sample `setup.cfg` file, which is based on the `setup.cfg` for the
`python-openstackclient` project:

.. code-block:: ini

    [entry_points]
    console_scripts =
        openstack = openstackclient.shell:main

    openstack.compute.v2 =
        host_list = openstackclient.compute.v2.host:ListHost
        host_set = openstackclient.compute.v2.host:SetHost
        host_show = openstackclient.compute.v2.host:ShowHost

This will register three commands - ``host list``, ``host set`` and ``host
show`` - for a top-level executable called ``openstack``. To document the first
of these, add the following:

.. code-block:: rst

    .. autoprogram-cliff:: openstack.compute.v2
       :command: host list

You could also register all of these at once like so:

.. code-block:: rst

    .. autoprogram-cliff:: openstack.compute.v2
       :command: host *

Finally, if these are the only commands available in that namespace, you can
omit the `:command:` parameter entirely:

.. code-block:: rst

    .. autoprogram-cliff:: openstack.compute.v2

In all cases, you should add the following to your `conf.py` to ensure all
usage examples show the full command name:

.. code-block:: python

    autoprogram_cliff_application = 'openstack'

__ http://www.sphinx-doc.org/en/stable/extdev/markupapi.html
__ https://docs.python.org/3/library/fnmatch.html
