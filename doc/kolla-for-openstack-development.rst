=====================================
Using Kolla For OpenStack Development
=====================================

Kolla-ansible can be used to deploy containers in a way suitable for doing
development on OpenStack services.

.. note::

    This functionality is new in the Pike release.

Heat was the first service to be supported, and so the following will use
submitting a patch to Heat using Kolla as an example.

Only source containers are supported.

.. WARNING::

   Kolla dev mode is intended for OpenStack hacking/development only. Do not
   use this in production!

Enabling Kolla "dev mode"
-------------------------

To enable dev mode for all supported services, set in ``/etc/kolla/globals.yml``:

::

    kolla_dev_mode: true

To enable it just for heat, set:

::

    heat_dev_mode: true

Usage
-----

When enabled, the source repo for the service in question will be cloned under
``/opt/stack/`` on the target node(s). This will be bind mounted into the
container's virtualenv under the location expected by the service on startup.

After making code changes, simply restart the container to pick them up:

::

    docker restart heat_api

Debugging
---------

``remote_pdb`` can be used to perform debugging with Kolla containers. First,
make sure it is installed in the container in question:

::

    docker exec -it -u root heat_api pip install remote_pdb

Then, set your breakpoint as follows:

::

    from remote_pdb import RemotePdb
    RemotePdb('127.0.0.1', 4444).set_trace()

Once you run the code(restart the container), pdb can be accessed using
``socat``:

::

    socat readline tcp:127.0.0.1:4444

For more information on remote_pdb, see
https://pypi.python.org/pypi/remote-pdb.
