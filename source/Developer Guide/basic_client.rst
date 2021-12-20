Basic Client
============

Koji's language is Python, so also basic client is best done in this language.
Anyway, XMLRPC API is available for anyone. Our python client has some
advantages like retrying failed calls, supporting ``nil`` extension and large
integers (``<i8>`` tag).

.. code-block:: python

    import koji

    mytag = "mytag"
    session = koji.ClientSession("https://koji.fedoraproject.org/kojihub")
    try:
        repo_info = session.getRepo(mytag, koji.REPO_STATES["READY"], dist=True)
        if not repo_info:
            print(f"There is no active dist repo for {mytag}")
    except koji.GenericError:
        print(f"Tag {mytag} doesn't exist")

What you can see are these main points:

- We're not using basic xmlrpc library but ``ClientSessions`` wrapper with few
  benefits
- Only required parameter is hub's URL.
- koji has a lot of constants which should be used instead of numeric values.
  They can be found in the `koji/__init__.py
  <https://pagure.io/koji/blob/master/f/koji/__init__.py>`_
- Normal python exception handling applies with ability to catch standard koji
  exceptions.


Multicalls
----------

Typically for queries API overhead is big, so using XMLRPC's multicall feature
is something developer can benefit from. Using it is described :ref:`here
<multicall>`.
