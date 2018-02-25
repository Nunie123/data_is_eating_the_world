Title: Building a Data Visualization App with Flask and React (Part 1 - Defining Requirements)
Date: 2018-02-24
Category: Visualization Web App
Tags: python, flask, javascript, react, data visualization, data analysis, requirements
Slug: build-vis-app-part-1
Author: Ed Nunes
Summary: This is the first part of a blog series on how to go about building a data visualization web application using a Flask API backend and React frontend.  I will discuss how to gather requirements.

## Introducing this Article Series

In this series of blog posts I am going to demonstrate how to create a web application from the design phase all the way through deployment.  I have chosen to make this application using [Flask](http://flask.pocoo.org/), a backend Python web framework, and [React](https://reactjs.org/), a frontend Javascript web framework.  While much of the information in this series is going to be useful for anyone interested in the process of building a web application, there are going to be many parts that dig into the particularities of the technologies I'm using.  If any post is going to just deep dive into the code for a particular technology I'll give you a heads up beforehand.

You can see the code for this application at: [https://github.com/Nunie123/narratus]

## The First Step: Gathering Requirements

When building any application the first step is gathering and recording requirements.  There's no way to build an application unless you know what the application is going to do.  And having a general sense of what you want to build is not sufficient ("_It'll be like Facebook, except I'll be the billionaire instead of Zuckerberg_").

Coding is very much a detail-oriented practice.  While you don't need to plan out every function and every variable, you do need a list of features describing what the application should be able to do.  This list is called the [requirements](https://en.wikipedia.org/wiki/Software_requirements_specification) because all of the included features are required before the application can be considered complete.  It can sometimes be helpful to frame them as [User Stories](https://en.wikipedia.org/wiki/User_story)("_As a User, I can view all of the other users I have identified as my friend in a single view_").  

Frequently you will need to go to other people to find out the requirements (thus the __gathering__ of requirements).  If you work on a professional software development team you will likely have a [Product Owner](https://www.scrum.org/resources/what-is-a-product-owner) that will be in charge of defining the requirements.  On smaller teams or working by yourself you'll have to gather the requirements on your own.  In gathering requirements it's important not to just __think__ about what the user wants, but to go out and actually talk to prospective users to find what is valuable.  There have been many well built applications that were complete failures because they were based on features users didn't actually want.

There is a good chance that you will be unable to build an effective application without soliciting user feedback.  If you are planning on making a commercial product, this is critical.  If you're doing a personal project to explore a new technology or practice your skills, build an application that you will use.  Now you have easy access to a prospective user: yourself.

When I compiled the requirements for the application I'm building in coordination with these blog posts, called [Narratus](https://github.com/Nunie123/narratus), I knew the intended users are the members of the data engineering team I work on.  Our team has had many group discussions about improved tooling to increase our productivity.  When preparing for this project I had a long brain-storming session with our Team Lead about the tooling we wish we had to do our jobs.  I took notes and distilled that conversation into the requirements document shown below.

## Level of Detail in Requirements Documents

Getting the right level of detail in the requirements is a balancing act.  If you provide too little detail then you leave too much decision making about requisite functionality at the discretion of the person writing the code.  Even if it's the same person writing the code that defined the requirements, in the middle of a coding session is the wrong time to be thinking about what features should be implemented.  Even if you do an excellent job defining your requirements on the fly, you'll have to [context switch](https://en.wikipedia.org/wiki/Context_switch) to put your requirements gathering hat on, which is a recipe for wasted time and worse code.

Too much detail has its own problems.  First, it means you'll be spending an awful lot of time writing requirements before you can get down to writing code.  In (Waterfall application development)[https://en.wikipedia.org/wiki/Waterfall_model] it is typical to go into a great amount of detail about everything that will be built and how it will work.  In theory this doesn't seem so bad, as it gives management an opportunity to approve the work to be done and the developers don't end up making independent design decisions that conflict with each other.  In practice, however, too much detail can lead to problems (which is why the Waterfall Model has given way in most companies to [Agile development](https://en.wikipedia.org/wiki/Agile_software_development)).  These problems usually occur because the requirements will often need to be changed over the course of development, and the Waterfall model does not handle these changes well.

## Requirements Change

While gathering the requirements is a critical initial step in building an application, do not assume that the process of gathering your requirements has ended.  As mentioned briefly above, expect your requirements to change.  If you're working on a professional development team you should be seeking feedback from your users throughout the project: showing them prototypes and beta versions or asking clarifying questions.  This will frequently result in requirements being added, changed, or dropped.  This iterative process of refining an application based on user feedback is a central tenet of Agile software development.  

In addition to requirements changes from users, there may be changes that result from the development process itself.  It could be that in the process of writing the actual code you realize features that probably should have been included ("_Is there a reason we are only putting a logout button on half of the views in our application?_").  Or you may find that a requirement is too vague to be implemented ("_What do you mean by 'The user should be able to talk to the application'?_").  It's also common to find that what appeared to be a small feature will end up more than doubling the complexity of the application ("_Adding an AI Chatbot to our flashlight application seems like a cool feature, but maybe we'd prefer to ship within the decade, instead._").  

_Well if we're just going to change the requirements then what's the point?_  The point is that you need some way to define what you will be coding and when you will be done.  While you can and should make changes, those changes should be done with careful deliberation and, ideally, lots of research.  Remember that software is for the users, so their needs should always be foremost when thinking about requirements.

## Gathering Requirements for Narratus

Like all good applications, I came up with the idea for Narratus when I recognized a problem and wanted to explore how an application might address that problem.  One of the core responsibilities of my team at work is to perform analyses on the data generated by another team's application and then turn that analyses into automated reports.  I noticed that our team is pretty good at transforming the data and performing analyses, but was not as productive as I would hope when translating those analyses into informative and visually appealing reports.

Right now we have a custom python application (a version of which has been [published on GitHub](https://github.com/speedyturkey/porthole)) that executes predefined SQL queries against our [Data Mart](https://en.wikipedia.org/wiki/Data_mart), pipes that data into custom HTML/CSS/JavaScript files which are printed to PDF and emailed.  While this application has served us well, and was quite an achievement for us given the constraints we were working under when it was developed, it has some fundamental shortcomings that suggest a complete rewrite is warranted rather than incremental improvements.

First, the methods of report publication are inherently limited.  The application can save reports to the server or email them out.  Because there is no [GUI](https://en.wikipedia.org/wiki/Graphical_user_interface), there is no way to display reports on the internet, our company intranet, or locally.  Despite using HTML to generate the reports, the final reports are only available in PDF (or as Excel files if there are no visualizations).  

Second, our process for generating visualizations through custom JavaScript is slower than we would like.  We are known as the Data Team, so perhaps its not surprising that we're not rock star JavaScript developers.  However, data visualization is a core function of our team.  The faster we are able to generate and iterate on visualizations and reports, the more productive we will be.  While we have looked at various BI tools to help with visualization, including a long look at [Tableau](https://www.tableau.com/), we decided that the tools available didn't quite meet our needs.  Given the expense (we were quoted around $20k/year for Tableau, which has similar pricing to the rest of the BI industry), we were reluctant to get [Locked In](https://en.wikipedia.org/wiki/Vendor_lock-in) to a product that only did 90% of what we needed.

Finally, our queries, visualizations, and reports could use some better organization.  We produce a lot of SQL files, but we don't have a convenient way to browse and tag our files.  We have a lot of reports that are often rewritten, abandoned, and reinstated.  It sometimes be a bit of a process to figure out which code goes with which report, and which reports are currently being published.  Right now we mostly convey this through a combination of file structures, naming conventions, SQL tracking tables, and memory.

Our team has had a series of conversations about what sort of tooling improvements would allow us to be more productive.  Those conversations as well as a detailed conversation I had with our Team Lead constituted the research I did to build the below requirements document.

## Narratus Requirements

The requirements document resides in the Narratus repo [here](https://github.com/Nunie123/narratus/blob/master/requirements.md).

1. Report Generation
    1. Query databases to retrieve data.
    1. Create Excel file from tabular data.
    1. Create charts from data.
    1. Compose charts and tables into pdf document.
    1. Compose charts and tables into html document.
    1. Automated report generation.
    1. Logging.
1. Report Publication
    1. Publication types:
        1. Email with attached report.
        1. Email with embedded report (e.g. embedded html email).
        1. Download to user's computer.
        1. Update website.
    1. Recipient types:
        1. Passive - Recipient receives email reports controlled by administrator.
        1. Active - Recipient can access application to download or subscribe to reports.
    1. Automated report publication.
    1. Logging.
1. Data Exploration
    1. UI interface to create charts from query results.
    1. Charts design updated with existing dataset.
    1. Chart design persists for new query results.
    1. Queries and charts easily saved.
1. Code Organization
    1. Reports saved in one location and categorized.
    1. Display report attributes:
        1. Recipients.
        1. Visualizations.
        1. Queries.
        1. Update/publication schedule.
        1. Publication method(s).
1. Copy Data Between Databases
    1. Select source and destination databases, schemas, and objects.
    1. Schedule regular updates.
    1. Specify updating all data or only new records.
    1. Logging.
1. Update Database Objects
    1. Update derived tables from copied data.
    1. Add indexes to copied and derived tables.
    1. Logging.
