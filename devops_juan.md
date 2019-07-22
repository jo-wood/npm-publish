# Juan Gonzalez - Continuous Integration

* build a RESTful API for a couple of tables
* 1 table records
* the other table for genre grouping

* how long will it take? best case scenerio because not being given ANY constraints right now
  * depends (this is brand new API, nothing to migrate with, and we are the developers, how do we esimate our time?)

* software estimation - tunnel vision as developers
  * 'need to build some routes, the database is up to me'
  * 'need buy a server to actually host this database and routes'
  * as we estimated our time to write 'simple' routes for our gets/posts, but we didn't consider even 30 mins in adding seed data. then the 30min to build the SQL query (how did we even decide to use SQL?)
  * client says day off, send me the email - I can't send you there it's on my machine?
  * I have only tested it here in the dev on my local server
  
* developer says 'will take me 4 days' - manager tells client will take 5 weeks...
* because the manager knows how many interactions, meetings, team dependencies are going to add to us getting our product complete

* Book - Mythical Man Month
  * refers to the time in software estimates when how long something would take was predicted by how many *man* *months* it would take
  * then what managers started doing is (okay so it will take 16 man months, so I can do 8 man months with more people)
  * but the problem is that many tasks need to be complete by ONE person - we need to understand the PROCESS
  * process versus, how long I personally as a dev estimate the task to take me
  * Juan, with his experience doing the task sooo many times, has a very good idea of how long it would take HIM
  * but now I'm working with a new client and a specifc task
  * Juan recommends taking a log of how long tasks are taking you (because you can never truly remember how long the task took the first times you did it - because 15 mins is a millisecond in a dev's world)
    * what kind of artifact (schema? route? html template? queries?)
    * and the time in units to complete
  * then once in a while aggregate this data
  * can find out what you are particularly good at (what's consistent in how long the task takes you)

## What do we typically miss

* schema
* configuration
* merges
* integration workflows (how to the next team's machine)
* infastructure (servers in the right place)

## Code example

* a mongo database
* multiple data migrations
  * xxxxx_first_org.js
  * xxxxx_user_experience.js
  * xxxxx_first_review.js
  * xxxxx_first_insights.js
* need to make sure everyone is seeding from the same data to have the correct expected outcomes
* first_org migration - 27 lines of code
* IBM's official metric in the 80's (30 lines of code a DAY)
* first migrate took an hour
* second one took 5 mins...
* so do lines of code really matter?

* (in log, wouldn't say, i can do 10 migrations in 2 hours, should say, i can do a migration factory in 2 hours)*

* graphQL - using types for routes
  * and graph QL puts those types together and figures out what query it needs to be making
  * no limit to the endpoint options, but dev needs to set these queries specifically based on needs
  * you are setting what your api can tell others (such as when we look at an API doc) - the retriever can say, what do you know? you know persons? what do you know about persons? the API can specify it knows places, now the retriver can say okay I want persons for a specific place

* mongo aggregration pipelines
  * each query is filtering data in and out - and then by the end will have the 1 piece of info needed, and pass this on to graphQL

* this project took 2 coding sessions (never more than 3 hours at a time)
* but took 2 months to plan

## DevOps

* this term isn't always used correctly
* so not everywhere will follow this, but this is the take on DevOps

* as developers, what do we need in order to timebox, is do the coding (that's DevOps)
* to start this journey, the log will help you be more repeatable and accurate, and trustful
* anything that developers need to do to operationalize the process

* how do we make our development process well defined and repeatable, and predictable

#### Dependencies + Config

* writing code, but realizing that all this code requires a specific db
* here, I need Mongo and it needs to be in the format that my code is using
  * collection of assessments, and finding `code` and `title` cols
* some could say, well that's not my problem, I wrote what my code needed to accomplish, but this is useless
* healthy software teams need to build everything up around your code
* if i want this to work, i need to make sure
* i will write a migration, that has those 2 fields, if my migration can run, then people can use this code (now I'm taking care of teh dependency)
* and EVERYTHING that code needs as a dependency in order to run
* you in your machine, need mongo
* so while you're developing, I need to make it clear that you need Mongo, or in my script i need to be checking that you are in fact RUNNING mongo

#### Code/Config + Version -> Builds

* just like your code can change, your migrations can change
* your scripts could change
* this is what is a build (code configuration)
* now I have a new version, where my code now has half the migration complete, and then build
* so that means we are building everytime we version through our configuration changes

#### Build + ENV -> Release

* of course will need to move from my local Mongo, and move this code to wherever the true Mongo DB exists
* worked locally when there is nothing here and I build
* but teh configuration locally is NOT necessarily the configuration of the integration server
  * ESPECIALLY not the same as the production env
* our release comes from a build, and this build is able to move through the different configuration changes
* our build moves through, sees that we are being given a new db environment, and this data is good, okay i will use this instead of my migrations
* therefore to test this, need to run my build through all of these environments and configurations
* not good to have 1 build for developement, and then say, i just need 1 more commit before I push it to the production
* should always be **ONE BUILD**

#### Release + Process -> Deploy (stateless)

* you never know which system your production server is going to be running (will it be linux? OS? )
* once you have a release that is has a build that passed every stage
* that build now has a 'wrap' around it that allows it to work in any machine you put it in
* are able to put it in any process
* ie, this is what docker would do for you
  * take less time setting up that 'wrap'

* these are the phases that we need to be able to explain that are required in developing the software

#### npm

* npm allows you to run this type of workflow
* you have your scripts in your json
* ie. whenever I do a build
* therefore every time we are committing, these scripts we made sure were set, allows us to be constantly checking that our build is working 

```json
"scripts" : {
//ie
  "startdev": "",
  "start": "",
  "build": "",
//ie
  "lint": "./n_m/.bin/eslint src",
  "fix": "./n_m/.bin/eslint src --fix",
  "test": "mocha",
  "babel": "./n_m/.bin/eslint src -d dist",
  "build": "./n_m/.bin/eslint src -d dist --minified",
  "pretest": "npm run fix",
  "prebuild": "npm test",
  "postbuild" : "npm version patch && git status ."
}
```
