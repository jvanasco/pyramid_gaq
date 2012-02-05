IMPORTANT

i've replaced this package with gaq_hub, which consists of a generalized 'hub' object, and helpers for pyramid and pylons to manage it.

this package will no longer be maintained

the 0.0.6 version it is largely backwards compatible with this release, with a few minor changes:
	1. gaq_print() was renamed to gaq_as_html()
	2. "from pyramid_gaq import *" is now "from gaq_hub.pyramid_helpers import *"


gaq_hub on github
	https://github.com/jvanasco/gaq_hub
	
gaq_hub on pypi
	http://pypi.python.org/pypi/gaq_hub


=================
=================

pyramid_gaq gives lightweight support for Google Analytics under pyramid

it creates and manages a _gaq namespace under request.tmpl_context, which can be updated in handlers and templates, and printed out -- in the correct order -- via a helper function

if you're just using _trackPageview, this package is likely overkill 

but if you're using any of this functionality, then its for you:
- custom variables for performance analytics 
- event tracking for backend interaction / operations 
- ecommerce tracking 
- rolling up multiple domains into 1 reporting suite

This package lets you set GA code wherever needed, and renders everything in the 'correct' order.

Every command has extensive docstrings, which also include, credit, and link to the relevant sections of the official GoogleAnalytics API docs.


# Supported Concepts & Commands

* Core
** Choice of using a single , queued, "push" style command - or repeated ga.js API calls
** _setAccount
* Multiple Domain Tracking
** _setDomainName
** _setAllowLinker
* Custom Variables
* _setCustomVar
* eCommerce
** _addTrans
** _addItem
** _trackTrans
* Event Tracking
* _trackEvent

# Pylons and Pyramid

if you're using pylons, there is also a pylons_gaq distribution that does the exact same thing.

https://github.com/jvanasco/pylons_gaq
https://github.com/jvanasco/pyramid_gaq


# QuickStart

## import this into your helpers

Dropping it into your helpers namespace makes it easier to use in templates like mako.

lib/helpers.py

    from pyramid_gaq import *
    

## configure your BaseController to call gaq_setup on __init__

This example is from my "pylons style hander".

There are only two vars to submit:

1. Your Google Analytics Account ID
2. Whether or not your want to use the "Single Push" method, or a bunch of separate events.

handlers/base.py

	class Handler(object):
	    def __init__(self, request):
        self.request = request
        h.gaq_setup(request,'GA_ACCOUNT_ID',single_push=False)


## When you want to set a custom variable , or anything similar...

Under Pyramid, all the gaq_set* commands also accept an optional kwarg for the response object.  If you don't submit it, it will call pyramid.threadlocal.get_current_request 

	h.gaq_setCustomVar(1,'TemplateVersion','A',3)
	h.gaq_setCustomVar(1,'TemplateVersion','B',3,request=self.request)

If you're calling this from a handler, 'request' is easily available this syntax is quite readable:

	h.gaq_setCustomVar(1,'TemplateVersion','A',3,self.request)
	h.gaq_setCustomVar(2,'Author','Me',3,self.request)

If you're calling from a template, the request object is a little removed, so this syntax can keep your code pretty :

	h.gaq_setCustomVar(1,'TemplateVersion','A',3)
	h.gaq_setCustomVar(2,'Author','Me',3)
	
	
## To print this out..

In my mako templates, I just have this...

	<head>
	...
	${h.gaq_print()|n}
	...
	</head>

Notice that you have to escape under Mako.   For more information on mako escape options - http://www.makotemplates.org/docs/filtering.html
