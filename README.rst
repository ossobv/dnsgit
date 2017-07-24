dnsgit :: Keep track of your DNS history using a Git repository
===============================================================

*Keeping track of your DNS record history is now as easy as running a
cron job.*

If you manage a couple of hundred domains which can be edited using a
web interface, it may come in handy to have history.

As OSSO, we use `PowerDNS <https://www.powerdns.com/>`_ to keep track of
customer DNS records. Along with its SQL backend, it works rather well.

However, it lacks history/undo. That's where *dnsgit* comes in:

**On a daily basis, it runs AXFR queries on the nameserver and writes
the current state to a local Git repository. The state is
sorted/normalized, so only real changes are noticed/stored.**

Any undo operation still has to be performed manually, but at least
you'll know what to undo.


Example setup
-------------

.. code-block:: console

    # install -T -o755 dnsgit /usr/local/dnsgit
    # mkdir /srv/dnsgit
    # cd /srv/dnsgit

    # git init; touch README.rst; git add README.rst; git commit -m Initial

    # dnsgit update-all

Run that last one -- ``dnsgit update-all`` -- from a daily cron job.

This will initialize and update the local repository and keep track of
changes.

For instance, a change may look like this:

.. code-block:: console

    # git show fb8c68e3522c36ad4aa90dd21553c812c7fd0de1
    commit fb8c68e3522c36ad4aa90dd21553c812c7fd0de1
    Author: root <root@ns1.osso.nl>
    Date:   Wed Jul 5 00:25:31 2017 +0200

        changed: osso.io.

    diff --git a/o/osso.io. b/o/osso.io.
    index 7ed831e..03c5477 100644
    --- a/o/osso.io.
    +++ b/o/osso.io.
    @@ -1,5 +1,5 @@
     osso.io.               86400 IN SOA ns1.osso.nl. info.osso.nl. (
    -                               2017061604 ; serial
    +                               2017070400 ; serial
                                    14400      ; refresh (4 hours)
                                    3600       ; retry (1 hour)
                                    604800     ; expire (1 week)
    @@ -76,7 +76,7 @@ production.osso.io.        3600 IN A 217.11.222.333

     rapid.osso.io.         86400 IN A 185.11.222.333

    -stats.osso.io.         86400 IN A 185.11.222.300
    +stats.osso.io.         86400 IN A 185.11.222.400

     stats2.osso.io.        86400 IN A 185.11.222.500



License
-------

dnsgit is free software: you can redistribute it and/or modify it under
the terms of the GNU General Public License as published by the Free
Software Foundation, either version 3 of the License, or (at your
option) any later version.
