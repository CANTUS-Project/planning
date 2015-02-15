CANTUS Client Application: The CANTUS App
===============================================================================

Name
------------------------

To help users understand exactly what's going on here, I think we'll just call it "The CANTUS App."
It's unlikely anybody will be confused about what this is or does, and using such a straight-forward
name will help people to find it. In addition, saying "The CANTUS App" will help differentiate this
interface from "the old CANTUS website."

Requirements
------------------------

Paraphrasing the summary: The CANTUS App will be an HTML/CSS/JavaScript Web app that, at first,
merely replicates the existing search-and-retrieval functionality of the CANTUS Drupal website, but
with improved searching and a touch-screen-friendly user interface.

Functional Requirements:
- Must be written in HTML, CSS, and JavaScript only.
- Must interact with the CANTUS Server Application (Abbott) only through a RESTful, JSON-based API.
- Must be able to prepare and submit queries, format and display results, and connect with other
    CANTUS-network Web projects by itself, based on JSON data from Abbott.
- Must connect to other projects that allow viewing images of the book/manuscript/whatever, though
    these hyperlinks will be provided by Abbott.

Functional Restrictions:
- Must be capable of working effectively on touch-screen devices.
- Must hold a "clipboard" or similar, to facilitate a more useful replacement of browser tabs.
- Will not display images or allow browsing of source books.

Technical Requirements:
- Must use a JavaScript library that will facilitate deployment on mobile platforms.
- Demonstrable compliance with the CANTUS API.

Elaboration of Functional Requirements
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Must be written in HTML, CSS, and JavaScript only. This will not be difficult in itself, since there
are now a wide range of examples of database-like Web applications, and our client-side processing
will not be particularly interesting.

Must interact with the CANTUS Server Application (Abbott) only through a RESTful, JSON-based API.
This is also not particularly difficult, since we will have a clean and (eventually) well-known
description of the data protocol to use.

Must be able to prepare and submit queries, format and display results, and connect with other
CANTUS-network Web projects by itself, based on JSON data from Abbott. This is, more or less, the
very description of what the application does.

Elaboration of Functional Restrictions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Must be capable of working effectively on touch-screen devices. This will require some amount of
testing and consideration. I've never done this before, so it may prove to be quite a challenge.

Must hold a "clipboard" or similar, to provide a replacement for browser tabs. If we design this
well, it will be a domain-specific improvement on browser tabs, which are essentially little more
than a means to quickly switch context between a number of "pages." In fact, at first we can try
to simply replicate a tab-like experience (swipe from the top, a "chant drawer" slides down, and
you can switch your view to a different chant). Eventually we may be able to add features, like a
similarity comparison (some measure of how similar the melodies/texts of the chants are), or a
feature that finds all the common metadata of a group of chants, possibly allowing you to query for
other chants that share all those metadata.

Will not display images or allow browsing of source books. There are other Web sites and apps that
do this well, so it will be our purpose here to direct people to those sites and apps.

Elaboration of Technical Requirements
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Must use a JavaScript library that will facilitate deployment on mobile platforms. As it is, I
believe this restricts us to choosing between the libraries supported by Apache Cordova: jQuery Mobile,
Dojo Mobile, or Sencha Touch. Because sencha is delicious, I think that's what we'll go with. Okay,
it also looks like the solution that works most comfortably for me, and I'm writing this thing, so
that's how it's going down.

Demonstrable compliance with the CANTUS API. For which I think we can use Selenium WebDriver, and a
suite of automated tests in JavaScript.

Implementation
------------------------

I'm not sure how to add to "technical requirements" here. I'll have to figure this out a bit later,
once I already have a little bit of the server to interact with.
