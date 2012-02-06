============
User Stories
============

Overview
========
At this point in the process, we've discussed the application at length with our customer.

We've hammered out the specific requirements, now we need to plan out the project.

A nice technique for doing this is to break down the features into use cases and user stories.

Use cases help explain how the application works from a user perspective. User stories help modularize the application.

Bringing modularity to the process has many benefits:

It helps the customer prioritize features:
    Each user story can be seen as a single unit of functionality. It becomes easier to sort out the 'wish-list' features 
    from features necessary to meet the requirements. The customer can then, at a glance, contribute to the phasing-in of more
    specialized features. Something very powerful happens when the *customer* says without prompting "I really want this, but lets 
    save it for the next release".
    
It keeps scope in check:
    It becomes very easy to see, just by the sheer volume of stories, what the scope of the application really is.
    
    Well written stories are very autonomous. They don't rely on a specific implementation or other components (that's the goal but 
    exceptions are common)
    
    "Drive-by features" are minimized. The answer to any new ideas that come up after this stage is automatically "let me write up a user story". Then it gets worked into
    the backlog like any other feature. They're documented, agreed upon, and staged to be worked on. 

It provides a way to work more efficiently:
    Developers looking at the application as a series of modules helps expose ways to better meet the requirements independent of other features.
    
    Work can happen more readily in parallel. 
    
    Estimation becomes something that a normal person can do with confidence (Planning Poker helps here)
    
    
Use Case Diagrams
=================

Use case diagrams, from the UML standard, aren't terribly helpful with an application of this small size.

However, they are important to understand, so we'll provide some as examples.

.. todo::
   I need to add in some use case diagrams for this app.
   
User Stories
============

User Management
---------------
Sales Managers need to be able to manage users in the system. 

Contact Management
------------------
The system's primary purpose is to provide interfaces to manage contact information. 

Sales Staffers and Sales Managers need to be able to create and manage contacts. Contacts can be
grouped into contact lists. Contacts and contact lists are only visible to the person who
created them, and the Sales Manager, unless they have been shared with another Sales Staffer.

See :ref:`contacts-data` for required fields. Some fields (such as 'e-mail address') can
have multiple values, each value also gets a descriptive label.

Contacts can be 'active' or 'inactive', separating current from former customers. 

Organization Management
-----------------------
Like contacts, we would also like to manage information about businesses, or the organizations
that contacts are affiliated with.

Organizations have the same basic user interactions as contacts (including sharing), but they can be associated with
contacts, indicating membership of the contact in the organization.

See :ref:`businesses-data` for required fields. Some fields (such as 'primary contact') can
have multiple values, each value also gets a descriptive label.

Regions
-------
We need to be able to maintain a hierarchical list of regions we service, so we can associate contacts,
organizations, and contact lists with a given region. 

We want to be able to do things like associate a contact with Kansas, and have them come up
in a search of any of Kansas, North America, USA, Western Hemisphere, etc. 

Tagging
-------
Contacts and contact lists should be tag-able. Tags will need to be managed centrally. A 
tag is a simple phrase that helps classify the data. The tag taxonomy will be flat.

Search
------
All users need to be able to find contacts easily. At minimum, they should be able to search by
e-mail, name, phone #, geographical region, tags, and associated sales staff.

Mass E-mail
-----------
Any user of the system needs to be able to send a bulk e-mail, formatted as HTML, to 
an entire contact list, or the results of a search.

Print Labels
------------
Any user of the system needs to be able to print out labels from an entire contact list, or the
results of a search.

Administration Back-End
-----------------------
We need to provide a way for Administrators to access any settings, static lookup lists, or other advanced 
features of the system. Debugging interfaces, log viewers, etc would also live here.
