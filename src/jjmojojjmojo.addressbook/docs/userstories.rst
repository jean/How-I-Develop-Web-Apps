=================
Project Breakdown
=================

Overview
========
At this point in the process, we've discussed the application at length with our customer.

We've hammered out the specific requirements, there are a few more steps before we begin development.

    #. We need to give the customer more information about how the app will be put together
    #. We need to plan the project and get timelines in place.
    
Both needs can be handled by breaking down the features and functionality into smaller pieces, and writing them up in a language that the customer can easily digest.

We can utilize a couple of concepts borrowed from the Agile Movement and more traditional software development to to break down the features: **user stories** and **use cases** (respectively).

User Stories and Use Cases
--------------------------

User Stories help relay functional requirements information. They are written in the voice of the end user, and typically encapsulate a single functional *concept*.

Use Cases are far more limited in scope, typically written in a more technical fashion and are useful for documenting how a functional concept is actually *used*.

Both types of documentation accomplish the same goals: 
    
    #. **Modularize the functionality of the system.**
    #. **Convey information about the system that users and clients can understand**

They differ both in scope and voice, but also in how they break down the problem: 
    - User Stories tend to be a bit vague, leaving the process flow up to the developer
    - Use Cases tend to spell out more detail about how the user will interact.

    
In either case, it's important to point out that User Stories and Use Cases are intentionally devoid of most implementation specifics. 

This helps keep the documentation accessible to non-technical readers, and frees the developer to implement the best solution to meet the requirements.

That said, implementation details **will** need to be captured. That's where the next section, :doc:`implementation` comes into play.
    
.. seealso::
   
   *Requirements 101: User Stories vs. Use Cases*, by Andrew Stellman 
        http://www.stellman-greene.com/2009/05/03/requirements-101-user-stories-vs-use-cases/
        
   *Writing Effective Use Cases*, by Alistair Cockburn
        http://www.amazon.com/Writing-Effective-Cases-Alistair-Cockburn/dp/0201702258
        
   *Use Cases, 10 Years Later*, Alistair Cockburn 
        http://alistair.cockburn.us/Use+cases%2c+ten+years+later
        
   
.. todo::
   Find more sources and references for this information.

Benefits Of This Approach
-------------------------
Breaking down customer requirements into User Stories and Use Cases has many benefits. 

It helps the customer prioritize features
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Each user story can be discussed in terms the customer can understand. It becomes easier to sort out the 'wish-list' features from features necessary to meet the requirements. The customer can then, at a glance, contribute to the phasing-in of more specialized features. 
    
Something very powerful happens when the *customer* says without prompting "I really want this, but lets save it for the next release".

It keeps scope in check
~~~~~~~~~~~~~~~~~~~~~~~
It becomes very easy to see, just by the sheer volume of stories, what the scope of the application really is.

Well written stories are very autonomous; most can be staged for development at any time.

"Drive-by features" are minimized. The answer to any new ideas that come up after the initial sign-off of the generalized requirements is automatically "let me write up a user story". 

The new idea gets worked into the backlog like any other feature. Its documented, agreed upon, and staged to be worked on. 

Scope can even grow as the customers needs evolve --that's OK-- if everything is continually worked through the backlog, new needs and changes in scope are just  *handled*, with little intervention.

It provides a way to work more effectively
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Developers looking at the application as a series of modules helps expose ways to better meet the requirements independent of other features.

Work can happen more readily in parallel. 

Estimation becomes something that a developer can do with confidence (Planning Poker helps here). 

It all leads to less wasted time, less waiting for customers, and ultimately a better quality product.
    
    
Use Case Diagrams
=================

.. todo::
   Explain how Use Case diagrams are read and constructed.
   
Functionality Breakdown
=======================
In this section, each User Story is presented along with the Use Cases that the story implies.

We'll be using a simplified Use Case template, focusing on use case diagrams.

.. todo::
   Provide Use Case details - just doing diagrams for now.

User Management
---------------
Sales Managers need to be able to manage users of the system. Managed users will have roles associated with them, and basic contact information. For the purposes of authentication, a user name and password will be stored and managed by the system.

Use Cases
~~~~~~~~~
.. image:: uml/User_Management.*


Contact Management
------------------
The system's primary purpose is to provide interfaces to manage contact information. 

Sales Staffers and Sales Managers need to be able to create and manage contacts. Contacts can be
grouped into contact lists. Contacts and contact lists are only visible to the person who
created them, and the Sales Manager, unless they have been shared with another Sales Staffer.

See :ref:`contacts-data` for required fields. Some fields (such as 'e-mail address') can
have multiple values, each value also gets a descriptive label.

Contacts can be 'active' or 'inactive', separating current from former customers. 

Use Cases
~~~~~~~~~
.. image:: uml/Contact_Management.*

Organization Management
-----------------------
Like contacts, we would also like to manage information about businesses, or the organizations
that contacts are affiliated with.

Organizations have the same basic user interactions as contacts (including sharing), but they can be associated with
contacts, indicating membership of the contact in the organization.

See :ref:`businesses-data` for required fields. Some fields (such as 'primary contact') can
have multiple values, each value also gets a descriptive label.

Use Cases
~~~~~~~~~
.. image:: uml/Organization_Management.*

Regions
-------
We need to be able to maintain a hierarchical list of regions we service, so we can associate contacts,
organizations, and contact lists with a given region. 

We want to be able to associate a contact with a location, for example the city of Kansas City, Mo, and have them come up
in a listing or search for Missouri, The Mid West, North America, USA, Western Hemisphere, etc.

Use Cases
~~~~~~~~~
.. image:: uml/Regions.*

Tagging
-------
It should be possible to associate arbitrary phrases (tags) with Contacts, Organizations and Contact Lists. Sales Managers will be in charge of maintaining the list of tags, but Sales Staff will be able to add new tags without intervention from a Sales Manager.

The system should try to prevent users from adding duplicate or very similar tags.

Tags will be organized as a flat taxonomy.

Use Cases
~~~~~~~~~
.. image:: uml/Tagging.*

Search
------
All users need to be able to find contacts easily. At minimum, they should be able to search arbitrarily by
e-mail, name, phone #, geographical region, tags, and associated sales staff (and any combination). Users should be able to choose to only include Contacts or Organizations.

A faceted search, much like a traditional phone book should also be available, allowing users to drill down by first letter of last name, region and tags.

Use Cases
~~~~~~~~~
.. image:: uml/Search.*

Mass E-mail
-----------
Any user of the system needs to be able to send a bulk e-mail, formatted as HTML, to 
an entire contact list, a specific organization, or the results of a search.

Use Cases
~~~~~~~~~
.. image:: uml/Mass_E-mail.*

Print Labels
------------
Any user of the system needs to be able to print out labels from an entire contact list, or the
results of a search.

Use Cases
~~~~~~~~~
.. image:: uml/Print_Labels.*

Administration Back-End
-----------------------
We need to provide a way for Administrators to access any settings, static lookup lists, or other advanced 
features of the system. Debugging interfaces, log viewers, etc would also live here.

Use Cases
~~~~~~~~~
.. image:: uml/Administration_Back-End.*
