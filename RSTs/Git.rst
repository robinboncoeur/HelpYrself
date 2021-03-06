###################################
My Totally Unofficial Git Help Page
###################################

Stuff on here sort-of works, sort-of. I wrote this in Nov 2018. I'll do some testing of this process as I've already encountered issues I couldn't explain when I cerated this set of documents called HelpYrself. If it works for you, great. However, YMMV.

Update 18.11.17: Using KeysToLife as my test app. KeysToLife is a collection of emailed devotionals that are worth reading more than just once, so I'm saving them in this document.


01 - Local set-up
*****************

On my Mac, in the main user folder - sort-of like the "Home/Username" folder in Linux - there is an AllHelp folder containing subfolders for each product, as well as a folder in AllHelp/GitPages, where all build files end up. If a new product's users would benefit from some sort of help system for that product, the product get a folder in that AllHelp folder. Any product with users will have a repository for that product on GitHub.

Using as an example a document called "KeysToLife", the following approach more-or-less seems to get the job done. You will need two instances of Terminal running: one for Sphinx to build html from .rst files, the other, to talk to github.


02 - Sphinx Terminal
********************

In the first terminal, run::

   cd AllHelp/KeysToLife
   
   sphinx-quickstart
   
In the list of prompts, it will ask whether to separate source from build: I have selected No. Note:

   *where this approach suggested, the author said to choose Yes. It sort-of didn't work quite as I had anticipated, so I'm selecting No.* 

In order to have the build end up in the right place, I have the following entry in the Makefile::

   BUILDDIR      = /Users/robinhahn/AllHelp/GitPages/KeysToLife

In the newly created index.rst, change the title of the header as desired, then add the pages in the RSTs folder so that index.rst refllects what's in the RSTs folder::
   
    .. toctree::
       :maxdepth: 3
       :caption: Contents:


       RSTs/Introduction.rst
       RSTs/Des.rst
       RSTs/Eli.rst
       RSTs/Spurg.rst

Modify the conf.py for sphinx to use the ReadTheDocs template, which I find easily more useful than some of the others::

    html_theme = 'sphinx_rtd_theme'

    html_theme_options = {
        'canonical_url': '',
        'analytics_id': '',
        'logo_only': False,
        'display_version': True,
        'prev_next_buttons_location': 'bottom',
        'style_external_links': False,
        #'vcs_pageview_mode': '',
        # Toc options
        'collapse_navigation': True,
        'sticky_navigation': True,
        'navigation_depth': 4,
        'includehidden': True,
        'titles_only': False
    }

Make sure to remove an indenting copying and pasting this last configuration text might introduce, as python sees indenting as a means of marking blocks of code. After adding and editing your files to the RSTs folder, edit your index.rst to include all these files. 


03 - Github Terminal
********************

Online, at Github, in your github account, set up a new repository. This will have a URL::

   git@github.com:{myAccount}/KeysToLife

Locally, in the git terminal, navigate to::

   cd AllHelp/GitPages/KeysToLife
   
   git init
   
Note: 
   *Not sure if this git init-ed .git folder is a keeper, as it will not be used to upload files. However, this is how I set up the initial html folder, which needs to have a .git folder, as this is the working folder from whence files will be staged and uploaded to the GitPages.*
   
Next::

   git clone git@github.com:{myAccount}/KeysToLife.git html

   cd html
   
   git init
   
   git symbolic-ref HEAD refs/heads/gh-pages
   
   rm .git/index
   
   git clean -fdx
   
At this point, the folder is clean. 


04 - Sphinx Terminal
********************

Run a preliminary::

   make html 
   
to check for errors and to put the first lot of files for git to stage and put up in your repository's gh-pages.


05 - Github Terminal
********************

Run::

   git add -A
   
   git commit -am "committing help"
   
   git push origin gh-pages
   
On Github, go to that repository and click on the settings icon (right-most). Halfway down is the GitPages section. Click on the URL for the pages and your new website should display.

   
06 - From then on
*****************

In the Sphinx Terminal - cd AllHelp/KeysToLife::

   make html

In the GitPages Terminal - cd AllHelp/GitPages/KeysToLife/html::
   
   git add -A
   
   git commit -am "update note xxxx"
   
   git push origin gh-pages
   
Read, rinse, repeat. Terminal saves previous commands, so:

  * up-arrow three times, hit enter
  
  * up-arrow three times, edit comment, hit enter
  
  * up-arrow three times, hit enter

Easy-peasy.
