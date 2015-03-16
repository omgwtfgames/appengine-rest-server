Copyright 2008 Boomi, Inc.
All rights reserved.

Basic REST server for Google App Engine Applications using the builtin Datastore API.

For extended feature list, see http://code.google.com/p/appengine-rest-server/

========

For example client usage, see example.txt.

========


Getting Started
---------------

To use with an existing application:

    import rest
    
    # add a handler for "rest" calls
    application = webapp.WSGIApplication([
      <... existing webservice urls ...>
      ('/rest/.*', rest.Dispatcher)
    ], ...)
    
    # configure the rest dispatcher to know what prefix to expect on request urls
    rest.Dispatcher.base_url = "/rest"
    
    # add all models from the current module, and/or...
    rest.Dispatcher.add_models_from_module(__name__)
    # add all models from some other module, and/or...
    rest.Dispatcher.add_models_from_module(my_model_module)
    # add specific models
    rest.Dispatcher.add_models({
      "foo": FooModel,
      "bar": BarModel})
    # add specific models (with given names) and restrict the supported methods
    rest.Dispatcher.add_models({
      "foo" : (FooModel, rest.READ_ONLY_MODEL_METHODS),
      "bar" : (BarModel, ["GET_METADATA", "GET", "POST", "PUT"],
      "cache" : (CacheModel, ["GET", "DELETE"] })
    
    # use custom authentication/authorization
    rest.Dispatcher.authenticator = MyAuthenticator()
    rest.Dispatcher.authorizer = MyAuthorizer()

Online demo
--------------

The Boomi Demo App Engine application is a fully working example application (based on the Google App Engine Greeting demo application).

Available types: http://boomi-demo.appspot.com/rest/metadata

Greeting schema: http://boomi-demo.appspot.com/rest/metadata/Greeting

Greeting instances: http://boomi-demo.appspot.com/rest/Greeting

Greeting instances with filter: http://boomi-demo.appspot.com/rest/Greeting?feq_author=bob@example.com&feq_date=2008-11-03T00:21:19.080553

Data accessible in both XML and JSON format (input can be specified using the HTTP "Content-Type" header, output can be specified using the HTTP "Accept" request header, e.g. "application/xml" or "application/json")
