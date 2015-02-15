CANTUS Server Application: Abbott
===============================================================================

Name
-------

I suggest we call the server application "Abbott," which is a catchy name that appears early in the alphabet.
Considering the specific religious nature of objects in the CANTUS database, and the Server Application's role as the steward of that information, the term "abbott" makes sense to me.
Moreover, it's not used for a notable software project (just an IRC bot that happens also to be written in Python, though with Twisted rather than Tornado).
In any case, nobody would ever confuse the two projects, and if they do, reading the first "about" sentence should clear things up.

Requirements
------------------

From the summary: "[this program] will accept JSON queries, if
required send them to Solr and reformat its results, consult the Drupal
database, search databases of other projects, then return results in JSON."

Thus the functional requirements:
- inbound and outbound client-side communication is in JSON;
- query a Solr server that will have indexed the CANTUS network (and beyond?);
- communicate with the CANTUS MySQL server, using the pre-existing schemas created by Drupal;
- communicate with other databases in the CANTUS network using their HTTP API;
- allow fast client-side experience;
- produce hyperlinks to Web apps/pages that hold images of a chant;

Additional functional restrictions:
- will not serve static content;
- will not serve HTML pages at all (appearance is left to the client application);

Additional technical requirements:
- demonstrable compliance with the CANTUS API;
- failure tolerance for all databases (with reduced usefulness);
- deployable with minimal configuration;
- adaptable to different, but similar, project environments;

Elaboration of Functional Requirements
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Inbound and outbound client-side communication is in JSON. This format is still gaining popularity for similar tasks. JSON is text-based so we can easily cache results in a wide variety of ways. JSON is easy to manipulate from Python. JSON also allows us to do data manipulation in the server program, specifying a client-side-friendly API that provides data in the format that's fastest for client applications to parse.

Query a Solr server that will have indexed the CANTUS network (and beyond?) We don't have the resources or requirements to justify writing a purpose-built search engine, so we will use Solr, which has been proven capable of meeting our needs by its use in similar projects. Furthermore, because Solr indexes content, we can ask it to index all the databases in the CANTUS network, so they may all be searched with equal efficiency.

Communicate with the CANTUS MySQL server, using the pre-existing schemas created by Drupal. In addition to providing "search results," this application must also serve the content of the CANTUS Database proper. To avoid a risky conversion and switch-over stage, to allow team members to continue using the Drupal website to add new content, and to allow a painless fallback to the Drupal site for all purposes in case our new applications are undesirable, the new server application should try as hard as possible to serve CANTUS Database content from the existing Drupal-MySQL database.

Communicate with other databases in the CANTUS network using their HTTP API. Since we'll (hopefully!) be able to index all the databases in the CANTUS network, we will (hopefully!) be able to provide content from those databases too. This will be more difficult because we'll have to use their HTTP API rather than their database, meaning there is a greater chance of failure. However, we should be able to cache results to some extent---it's not as though these chants are changing on a daily basis!

Allow fast client-side experience. One of the problems with the existing Drupal database application is that users perceive it to be slow. Although much of the experiential speed will be the responsibility of the client application, we can also take measures to both allow the client to do less processing, and also to return partial data so that users can start browsing (for example) search results even before all the results are ready.

Produce hyperlinks to Web apps/pages that hold images of a chant. There are at least some extant
resources, like those of the CANTUS Ultimus project, that will allow us to provide clients with a
hyperlink directly to the page a chant is on, hopefully even to a more exact position of that chant
on the page.

Elaboration of Functional Restrictions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Will not serve static content. We know existing web servers are good at this, so page images, HTML, CSS, JavaScript, and other related assets can be provided by an incumbent server.

Will not serve HTML pages at all (appearance is left to the client application). Just to emphasize the last point---this specifically means that the HTML/CSS/JS CANTUS client application will also not be served by this new application.

Elaboration of Technical Requirements
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Demonstrable compliance with the CANTUS API. In addition to the usual "white box" automated test suite, we will develop a "de-coupled" "black box" automated test suite that interacts with the application only via HTTP. This suite will measure compliance with the CANTUS API, and can therefore be used for any future server implementation too.

Failure tolerance for all databases (with reduced usefulness). The program should be capable of deciding when to give up on getting its results from a database in a timely manner, and still be capable of providing users with some information from the remaining databases. These results will admittedly be less useful than if all the databases were functional, especially if the Solr server goes down. The application should probably also be capable of telling users (and administrators) that something has gone wrong.

Deployable with minimal configuration. This application will *access* databases, and possibly maintain a cache, but otherwise retain no "state." Therefore, it should be possible to write a script (e.g., Ansible playbook) that turns a brand-new virtual machine into a production-ready server environment with minimal input. Ideally, setup will only require entering the VM's IP address, hostname, and the name and email address of a Responsible Person.

Adaptable to different, but similar, project environments. I'm not sure the extent to which this will be feasible or even desirable. I do not intend to develop a framework---the program code required for this software will probably be highly specific to our deployment. However, if Someone In The Future wants to, for example, migrate the Drupal-MySQL database to something else, they should be able to do so without having to rewrite the entire program.

Implementation
-------------------

This is less concrete at the moment.

We're going to use Python 3.4 because I already know it, and there are better solutions in Python than in C++.
We're going to use Tornado because it seems like it should be faster than Django, and we won't really need the templating or database-interaction features that Django offers (weird, right?)
I haven't yet decided how we'll interact with the Solr server (direct HTTP queries or through a library).
To interact with the MySQL server, for the amount of this that will actually be required, I think it'll be fine to just use something lik SQLAlchemy and write it by hand.
Hopefully that can be done asynchronously too.

How shall the Solr database update itself with new and modified content?

I would also like to target the application to a specific operating system and version, rather than writing a generic Python app. This would allow us, for example, to use distribution packages for the Python dependencies, which are more likely to be updated with bug- and security-fix patches without breaking compatibility than we will have the energy to test. Moreover, hopefully I'll be able to deploy a Linux system with SystemD, which offers watchdog, daemon-restarting, and sandboxing.
