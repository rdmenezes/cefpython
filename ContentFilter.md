# ContentFilter object #

See [RequestHandler](RequestHandler.md).`OnResourceResponse()`.

## CEF 1 ##

void **SetHandler**([ContentFilterHandler](ContentFilterHandler.md) handler)

> The `handler` is a python class that implements
> one or all of the [ContentFilterHandler](ContentFilterHandler.md) callbacks.

> You must keep a strong reference to the [ContentFilterHandler](ContentFilterHandler.md)
> object, otherwise it gets destroyed and callbacks won't get called,
> CEF Python keeps only a weak reference to the [ContentFilterHandler](ContentFilterHandler.md)
> object.