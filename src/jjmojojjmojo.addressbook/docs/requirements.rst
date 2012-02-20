============
Requirements
============
This project serves primarily as a technical example, but it is
also a way for me to document my overall process for developing applications.

Here we will pretend the requirements for the project came from a customer, and 
illustrate the process of finalizing the requirements prior to the next step,
breaking out user stories.

Customer Request
================
Customer needs are often relayed to developers in very informal, ad-hoc ways.

Here's the example we will be working from, which I feel is pretty typical:

::
    
    
    To: jjmojojjmojo
    From: A. Customer <a.valued.customer@place.of.business.co.zz>
    Subject: Contact List Help
    
    JJ,
    Sales has been keeping our business contacts in a handful of excel 
    spreadsheets and it's getting hard to manage.
    
    We need a way to keep track of contacts, can we add something to 
    the intranet?
    
    Thanks,
    A.
    
Not a lot of detail, sure. In spite of that, this is probably enough for me to run off and build an application. 

However, experience has shown that for maximum efficiency, consistent success and a happy relationship with your customers, it's important to nail down the specifics. As early as possible.

This is where analysis comes in.

Analysis
========
Looking at the basic request, there are a few assumptions we can make:

    - they are used to a tabular representation of their data
    - there are common address fields that we can count on being there (first name, last name, phone number, e-mail, etc)
    - it should be web-based.

We'll assume I've worked for this company for a while, and I know a few things about how the sales department does their thing:

    - Sales contacts for each staff member are duplicated in a central worksheet for mass e-mail campaigns
    - Otherwise, sales contacts are typically not shared (each staffer is assigned to a number of clients they service exclusively), but some big clients are assigned to multiple staff.
    - The sales staff is growing - there's real business value in this rough business procedure being put online and streamlined.
    - They classify clients by industry.
    
At this point we're starting to sketch out the features in our heads:

    - We'll need a basic Address management UI (create, replace, update, delete)
    - We'll need a way for addresses to show up in two places
    - We'll need a way to manage industry classifications
    - We'll need per-contact permissions
    - We'll need user management of some kind

But there are still lots of questions that need to be answered.    

This is the point where some quick research is warranted. I would check out other contact management/address book systems. I'd call sales, or drop by, and take a look at the current spreadsheets. 

As a side goal, I'd make sure what my customer contact was asking for is what's really needed. 

.. tip::
   Experience has shown that often the problem someone asks you to solve is a symptom of a larger problem. Good analysis will help ferret out these sorts of issues.  

A few e-mail volleys are in order, posing some questions and concerns, such as:

    - How should we do authentication?
    - What inputs/outputs are needed (e.g. do they want to export to excel?)
    - What fields need to be captured?
    
        - Will there just be one possible value for each field? (should we have three 'business phone', 'home phone' 'mobile phone' fields, or a way to add any number of phone numbers and classify as needed?)
        - Will we need to indicate which one is preferred?
    
    - Will you want the initial data loaded from excel, or will it be entered manually?
    - What other features would be helpful?
    - Search features?
    - Fixed list classifications or would they be arbitrary?
    - Any sensitive data stored? (SSNs, medical info)
    - Any specific technological or standards restrictions?
        
        - Browser compatibility
        - Section 508
        - Hosting platform (server, back-end, etc)
        - Mobile
        
    - Any business processes (e.g. workflows) to model in the app?
    
Imagine we had some back and fourth, had some meetings, and got these answers:

    - We'll use the company's AD for authentication (*yes, I see how you might think that's ironic*)
    - There's no need for general import or export at this stage, but they would like to consider adding export to a format they can load into Thunderbird
    - The data will not contain anything sensitive.
    - There will be a fixed field list (see below), but a few fields will need to be one-to-many with an indication that one of them is preferred
    - There should be a flexible tagging system to classify contacts
    - They would like to be able to find contacts by name, e-mail, phone #, tag, and region (state, continent, hemisphere, etc)
    - They would like to send HTML mass-emails to an entire contact list, or a search result.
    - They would like to print address labels from searches and contact lists
    - The site should work in as many browsers as possible, but Sales uses FireFox, so that's our primary target.
    - No restrictions as far as section 508 or any other standards. Mobile access would be great but can be handled in a later version.
    - I can host and deploy the app however I want (they're providing a linux server); they do want to use their existing PostgreSQL database cluster to store the data.
    - They don't need full-blown workflow in the app, but they do have the concept of a "disabled" contact; someone who is no longer a customer. They do have a few processes they may want to incorporate later.
    
We also got a new feature:

    - They also need to tack contacts that are actually *businesses*. This isn't happening in any special way in the excel spreadsheets. Regular contacts get associated with a business. Mass e-mail and search features need to also work for businesses.
    
So, this is a lot. The next step is to turn these data points into a general requirements document. Something that the customer (and preferably Sales!) could sign off on.

.. tip::
    'Sign-off' implies a contract. This is a great use of this intermediate requirements document. 
    
    But even if the document isn't used as a contract, it's a great way to keep the scope in check. 
    
    Further, translating technical requirements into prose helps catch oversights or potential issues that might impede progress (*wait, how do we handle regions?*).

Finalized Requirements
======================

.. note::
   
   This section represents a write up of the requirements gathered above. I want to be clear that the way this section is formatted is in no way prescribing a specific format or organizational style. 
   
   This document is an expression in prose of what we feel the customer wants. It should be written in your (or your company's) voice, and organized in a way that fits your processes.


Summary
-------
The Sales department is having trouble managing their sales contacts. They would like to move to a centralized, web-based solution.

Vital Information
-----------------
:Customer Contact: A. Customer
:Sales Contact: BA Baracus, Sales Manager, (919) 867-5309
:Additional Contacts: Hannibal Smith, Sales Associate, (919) 555-0001
:DBA: H.M. Murdock, (919) 222-1111
:IT Manager: A. Contact
:Preliminary Due Date: December 21, 2012

Overview
--------
The basic idea of the project is a classic address book. Contact information for sales contacts and businesses, organized by a flexible tagging system, and associated with one or more Sales staff.

The goal is to let individual staff members manage their own clients, while allowing them to share contacts with other staff. All contacts will be centrally managed as well, by the Sales Manager. 

Unless the contact or business is shared in the system, information will not be shown to other sales staff, but can always be seen by the Sales Manager.

The contacts are used for various purposes beyond basic contact management. Specifically, they are used in notification campaigns. These are usually e-mail based, but sometimes a list of phone numbers is used for phone campaigns, and less frequently, address labels need to be printed off for mail campaigns.

User Roles
----------
In this document and subsequent documents, we will refer to users by their role in the system. So far, we've identified three roles:

Administrator:
    A technical user with full rein over every aspect of the system. This user would be in charge of adjusting application-wide settings, and any maintenance tasks that may develop.
    
Sales Manager:
    A user with access to every contact in the system. This user has the ability to manage users, tags, regions, and other central data used throughout the system.
    
Sales Staffer:
    A user who would only see contacts that they added to the system, or contacts that were shared with them.

Specific Features
-----------------
User Management:
    People need to access the system. Sales Managers will provide access and create accounts

Contact Management:
    Contact information will be managed in an intuitive way. A contact entry could be a business. A contact can be associated with a business. Contacts can be shared between Sales Staff. Contacts will have a fixed set of fields (see `Attachment 1 - Required Data`_), but some fields will be one-to-many, and can be added in an add-hoc manner with a preference indicated (e.g. phone numbers).
    
Contact List Management:
    Contacts will be grouped into lists. Lists are owned by a Sales Staffer, and can be shared like contacts. 
    
Search:
    All users will need to search for contacts. They should be able to limit the search by e-mail, name, phone #, geographical region, tags, associated sales staff. Only contacts that the user is allowed to see will show up.
    
Mass E-mail:
    At the search result, business, or contact list level, the user should be able to send an e-mail to all contacts. The e-mail will be HTML formatted but also fall back to plain text.
    
Mailing Labels:
    As with Mass E-mail above, the user should be able to print a PDF of mailing labels.
    
Classification:
    There will be a simple, flat taxonomy. A classifier will be a multi-word string that can be applied to Contact Lists, Businesses, and Contacts. A central management interface will be provided for cleanup, but redundancy will be handled in the 'tagging' user interface elements.
    
Regions:
    A centralized hierarchy of regions will be managed in the system. Regions are associated with Contacts, Businesses, and Contact Lists. 
    
Restrictions
------------
The following constraints are being placed on this project:

    #. We must use the existing PostgreSQL database.
    #. We must provide authentication via kerberos to the AD.
    #. There will be no sensitive data stored in this system.
    #. It must support FireFox 8+, but other platform support is desired.

Preliminary Technical Details
-----------------------------
A server will be provided by A. Customer running a version of Linux, with Python 2.7.x installed.

The database will be provided by A. Customer's DBA in the existing PostgreSQL cluster.

There are currently no plans for further scale-out at this time.

Attachment 1 - Required Data
----------------------------
Here are all of the data fields that are to be captured.

.. _contacts-data:

Contacts
~~~~~~~~
All multiple fields will provide a title for each field (e.g. you can identify the second phone number as 'home')

+--------------------------------+----------------------------------------------------+----------+
| Field                          | Description                                        | Multiple?|
+================================+====================================================+==========+
| Given Name                     | 'First' name - official                            |       No.|
+--------------------------------+----------------------------------------------------+----------+
| Family Name                    | 'Last' name                                        |       No.|
+--------------------------------+----------------------------------------------------+----------+
| Nick Name                      | Preferred name - displayed instead                 |       No.|
|                                | of given name if provided                          |          |
+--------------------------------+----------------------------------------------------+----------+
| Phone number                   | 10-digit phone number                              |      Yes.|
+--------------------------------+----------------------------------------------------+----------+
| Address                        | Mailing address                                    |      Yes.|
+--------------------------------+----------------------------------------------------+----------+
| E-mail                         | Electronic mail address                            |      Yes.|
+--------------------------------+----------------------------------------------------+----------+
| Photo                          | Head shot of the contact                           |       No.|
+--------------------------------+----------------------------------------------------+----------+
| Notes                          | Additional information                             |      Yes.|
+--------------------------------+----------------------------------------------------+----------+
| Gender                         | Male, Female, Unknown                              |       No.|
|                                |                                                    |          |
+--------------------------------+----------------------------------------------------+----------+

.. _businesses-data:

Businesses
~~~~~~~~~~

.. note:: 
   In the system, I think we will refer to 'Businesses' as 'Organizations'.

+--------------------------------+----------------------------------------------------+----------+
| Field                          | Description                                        | Multiple?|
+================================+====================================================+==========+
| Name                           | Name of the organization                           |       No.|
+--------------------------------+----------------------------------------------------+----------+
| Description                    | Detailed information about the organization        |       No.|
+--------------------------------+----------------------------------------------------+----------+
| Primary Contact                | Person to contact for various purposes (a contact) |      Yes.|            
+--------------------------------+----------------------------------------------------+----------+
| Logo                           | Company logo                                       |       No.|
+--------------------------------+----------------------------------------------------+----------+
| Street Photo                   | Photograph of the front of the building            |       No.|
+--------------------------------+----------------------------------------------------+----------+
| Address                        | Mailing address                                    |      Yes.|
+--------------------------------+----------------------------------------------------+----------+
| Phone number                   | 10-digit phone number                              |      Yes.|
+--------------------------------+----------------------------------------------------+----------+

