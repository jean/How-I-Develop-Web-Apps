==============
Implementation
==============

Overview
========
At this point, the requirements have been nailed down, broken up into manageable parts, and analyzed.

The last planning step before we start coding is to work out the implementation details.

The level of detail in this phase is variable. For some projects, a summary of technical approach is enough. For others, fine detail about language constructs and object definitions might be more appropriate, complete with process diagrams and pixel-perfect UI mockups. The level of detail can (and often will) vary from feature to feature, or story to story, or case to case.

The goal is to lay out a blueprint for building each piece of the application. You want to produce artifacts that developers can follow, and customers can understand. A developer should, given the project artifacts (user stories, use cases, implementation details), be able to build an application to meet the requirements with little guidance.

To meet those goals, the tone and voice should technical, but explanatory. Comprehensive but not necessarily exhaustive.

Rough mockups are sometimes useful when writing up User Stories and Use Cases, but they must be vague and shouldn't tie the story or case to a specific UI. 

Within implementation details, precise mockups are appropriate, even encouraged. Again, you're conveying to other developers and your customer *how* things will work. It's acceptable to introduce front-end designs at this point.

Remember to be specific about what target release or sprint the implementation details are for; the goal is to meet the requirements *just enough* with each development cycle. There will often still be enhancements or refinements that will be slated to be worked on later. 

There are several positive side effects of this process:

    - Typically, there are a number of ways to meet a given set of requirements; this process helps narrow in on the best way for the given time frame or release.
    
    
    - Further, discussing implementation details amongst developers fosters collaboration and helps expose engineering and planning errors.
    
    
    - It becomes easier to estimate. In a Planning Poker session, it can often be difficult to come to a consensus without first agreeing on an approach.
    
    
    - It's easier to hand off a feature to another developer (or even another team) if the bulk of the design is pinned down and the project artifacts are written up well.
    
    
    - Sprints go smoother because there are fewer possibilities for blocking due to waiting for customer input, taking time to research how to implement something, or general pondering of design. If implementation documentation is done well, almost all of the questions are answered before work begins, so the developers can focus on producing sound, well tested code.
    
    
    - The customer is encouraged to read implementation details and express their opinions or objections. This way they know what to expect in the finished product, they have a chance to catch oversights, and they can 'fuss' over small issues ('can we make it *green* instead of *blue*'; 'that should be a *checkbox*, not a *dropdown*'; 'this doesn't look anything like *amazon dot com*!') before major design decisions are finalized.
    
    
    - When implementation details are specific, they can be relied on as limits for what is implemented during a given sprint or target release (*'for iteration #1, we're going to store these values in a JSON file, in iteration #2, we'll be migrating to MongoDB'*)
 

General Technological Approach
==============================
Based on :ref:`finalized-requirements`, we have made some basic platform and approach decisions:

:Back-End Language: `Python <http://python.org>`_, version 2.6.6.
:Application framework: `Pyramid <http://docs.pylonsproject.org/en/latest/docs/pyramid.html>`_, version 1.3
:Data Store: `PostgreSQL <http://www.postgresql.org/>`_, version 9. We will use `SQLAlchemy <http://www.sqlalchemy.org/>`_ and `Alembic <http://alembic.readthedocs.org/en/latest/index.html>`_ for database abstraction and migrations.
:Packaging: The application will be distributable as a python egg, and will include a `zc.buildout <http://pypi.python.org/pypi/zc.buildout/1.5.2>`_ buildout to build the documentation, develop, test, and deploy the application.
:Documentation: Maintained via `Sphinx <http://sphinx.pocoo.org/>`_
:URL Strategy: We will be using url mapping.
:UI Approach: Javascript-driven (`Jquery <http://jquery.com/>`_), asynchronous.

Rationale/Expansion
-------------------
I've had good experience working with Pyramid and Python for this sort of web application. It provides a robust object mapping system that's easy to test. It has a access control system built-in that is easy to implement and robust in spite of being relatively simple.

SQLAlchemy is one of the nicest database abstraction libraries I've ever used. It will provide flexibility when it comes to changing back-end databases over time. Alembic will help manage the data model as it evolves. We're targeting PostgreSQL 9 since it's what our customer already has in place. We'll leverage sqlite to facilitate faster unit testing of the data model and business logic.

This is a modern application, it's ideal for more modern dynamic, asynchronous, browser-driven approach. JQuery makes this easy. In general, this will be implemented as a RESTful API, using JSON as the interchange format.

Implementation Details
======================

User Management
---------------
.. seealso::
   :ref:`user-story-user-management`
   

