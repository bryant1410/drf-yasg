*********************************
Describing authentication schemes
*********************************

When using the `swagger-ui` frontend, it is possible to interact with the API described by your Swagger document.
This interaction might require authentication, which you will have to describe in order to make `swagger-ui` work
with it.


--------------------
Security definitions
--------------------

The first step that you have to do is add a :ref:`SECURITY_DEFINITIONS <security-definitions-settings>` setting
to declare all authentication schemes supported by your API.

For example, the definition for a simple API accepting HTTP basic auth and `Authorization` header API tokens would be:

.. code-block:: python

   SWAGGER_SETTINGS = {
      'SECURITY_DEFINITIONS': {
         'Basic': {
               'type': 'basic'
         },
         'Bearer': {
               'type': 'apiKey',
               'name': 'Authorization',
               'in': 'header'
         }
      }
   }


---------------------
Security requirements
---------------------

The second step is specifying, for each endpoint, which authentication mechanism can be used for interacting with it.
See https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#security-requirement-object for details.

By default, a top-level `security` that accepts all the declared security definitions is generated.
For the example above, that would be :code:`[{'Basic': []}, {'Bearer': []}]`. This can be overriden using the
:ref:`SECURITY_REQUIREMENTS <security-definitions-settings>` setting.

Operation-level overrides can be added using the ``security`` parameter of
:ref:`@swagger_auto_schema <custom-spec-swagger-auto-schema>`.


-------------------------------
``swagger-ui`` as OAuth2 client
-------------------------------

It is possible to configure ``swagger-ui`` to authenticate against your (or a third party) OAuth2 service when sending
"Try it out" requests. This client-side configuration does not remove the requirement of a spec-side
:ref:`security definiiton <security-definitions-settings>`, but merely allows you to test OAuth2 APIs using
``swagger-ui`` as a client.

**DISCLAIMER**: this setup is very poorly tested as I do not currently implement OAuth in any of my projects. All
contributions relating to documentation, bugs, mistakes or anything else are welcome as an issue or pull request. The
settings described below were added as a result of discussion in issue :issue:`53`.

The settings of interest can be found on the :ref:`settings page <oauth2-settings>`. Configuration options are similar
to most OAuth client setups like web or mobile applications. Reading the relevant ``swagger-ui`` docmentation linked
will also probably help.
