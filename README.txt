pyramid_gaq gives lightweight support for Google Analytics under pyramid

it creates and manages a _gaq namespace under request.tmpl_context, which can be updated in handlers and templates, and printed out -- in the correct order -- via a helper function