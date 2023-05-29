# eaa_quickstart_guide

This is documentation project illustrating the use of sphinx document generator to generate the Enterprise Application Access (EAA) Quickstart Guide.
The content is same as in https://techdocs.akamai.com/eaa/docs/get-started-introduction

The documentation file structure is as follows:

source directory
|
|--- bookmark_app.rst (reStructuredText for Bookmark Application QuickStart Guide)
|
|--- web_app.rst (reStructuredText for Web (HTTPS) Application QuickStart Guide)
|
|---tcp_app.rst (reStructuredText for TCP-type Client-Access Application QuickStart Guide)
|
|___ index.rst (reStructuredText for top-level file for the QuickStart Guide)
|
|--- images folder (contains all of the .jpeg images for all the .rst files)
|
|___ conf.py (Python config file)
