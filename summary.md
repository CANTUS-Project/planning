CANTUS Planning Summary Document
===========================================
We'll keep this document up to date with brief descriptions of what we're
planning, and links to lengthier descriptions where possible.

At this stage, all the projects still require names.

Relationship to Existing Infrastructure
-----------------------------------------
We're going to try re-using the existing CANTUS Drupal database, eliminating
a risky conversion and "switch-flip" stage. In fact, we'll even retain Drupal
for CANTUS team members uploading data, so we don't have to write what
amounts to two websites. The option remains open to replace the CANTUS
contributor website in the future. As will become clear, the new applications
can be deployed alongside the existing Drupal website, thereby minimizing the
risk of potential negative outcomes.

Search Solution
------------------
We'll set up Solr on the Drupal database (though not connected to
Drupal---just the database). Solr is search engine softare already used by
similar projects, like SIMSSA. Many of the customizations required to deal
with Latin ecclesiastical documents will have been made already for SIMSSA,
so we can copy their work. Solr will definitely be flexible enough, though I
expect we'll be fine-tuning it for quite a while.

New Server Application
------------------------
Then we'll write a new server application. It will accept JSON queries, if
required send them to Solr and reformat its results, consult the Drupal
database, search databases of other projects, then return results in JSON.
Coordinating all these sources means there will be many places to introduce
failures and errors. The relative simplicity allowed by this application's
restricted focus will allow us to design for robustness and speed (at the
expense of features).

New User Application
-------------------------
Finally, the new user interface will be an HTML/CSS/JavaScript Web app. This
HTML app will interact only with the new server application, and only in JSON
(plus image files where relevant). The HTML app will know how to prepare
search queries, decide how to format results for display, and how to request
and display any other content in the CANTUS database, always communicating with
the new server application in JSON. At first, we'll try to replicate the
existing functionality as closely as possible, with the exception of making the
web app accessible for touch-screen users. Additional features and enhancements
can follow.

One of the most exciting parts of this HTML app approach is that all the
major mobile platforms support HTML/CSS/JS apps. (Well, sort of. For Firefox OS
HTML apps are the only choice; BlackBerry OS also supports them natively; for
Android, iOS, and Windows Phone we will probably need to use Apache Cordova as
a wrapper). In effect, this means we can use a substantially similar codebase,
for the website and all mobile platforms we choose to target. And for mobile
users who are unable or unwilling to install our platform app, they can simply
use the website, which will already be touch-friendly.

When it comes time to implement search-saving and the comparison tool,
we will hopefully be able to do this with modifications only to the HTML
app.
