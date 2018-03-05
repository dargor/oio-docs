======================
Rebuild a rawx service
======================

Preparation
~~~~~~~~~~~

Find information about the service you want to rebuild.
By running ``openio cluster list rawx`` you will get the list of all rawx service ids accompanied by their volume path.

Verify that the service was automatically scored to zero by running ``openio cluster list rawx``.

If not, Lock the score of the targeted rawx service to zero by running ``openio cluster lock rawx <RAWX_ID>``. Where RAWX_ID is the network address of the service (ip:port).
This will prevent the service from getting upload requests, and will reduce the number of download requests.

Set the incident date
~~~~~~~~~~~~~~~~~~~~~

Set an incident on the target rawx service by running the ``openio volume admin`` command:

  .. code-block:: console

     # openio volume admin incident <RAWX_ID>

By default, the incident date is the current timestamp. You can tune this incident date by using the parameter ``--date <TIMESTAMP>``.

Check that the incident is actually set:

  .. code-block:: console

     # openio volume admin show <RAWX_ID>

     +---------------+-----------------+
     | Field         | Value           |
     +---------------+-----------------+
     | volume        | 10.0.0.186:6004 |`
     | incident_date | 1484517814      |
     +---------------+-----------------+

Launch rebuilding
~~~~~~~~~~~~~~~~~

You can now launch the rebuild by using the ``oio-blob-rebuilder`` tool:

  .. code-block:: console

    # oio-blob-rebuilder <NAMESPACE> --volume <RAWX_ID>


