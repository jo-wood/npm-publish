# Intro to AWS

Amazon Web Services

## What is AWS

* Why does Amazon provide these services?
* started from Amazon trying to turn their own ops into a platform
  (read some Steve Yegge)

* were trying to direct teams to share their info through APIs since so much cross-work
* was started to improve their own work flow, then saw the value of making public APIs

## Why AWS

* Tradeoff; your time & learning for money -- paying Amazon to be your sysadmins
* Flexibility in scale -- it doesn't make sense for you to buy a whole server/data center
* Take advantage of economies of scale

**How does it compare?**

* Heroku -- built on EC2

* value can be seen in as you are growing and scaling your app, allow AWS to automate more of this for you, and maintainining those automations
* depends on also how your company is strategized, are you already a tech company or are you just strictly providing a service that you just need to run well within it's domain of functionality

* looking at ec2 pricing - not all regions that you want to run within are going to have the same storage availability or even the right service
* even the massive drives, could add up quickly, but if only needed to compute for an hour and then never again this could be very useful

* may be valuable when you have many services that you are requiring and now you can have it all happening within AWS

## Using AWS

* Amazon API are designed pragmatically, though complex, they have better documentation than say Google APIs in understanding how to do what you want (more complete)

* AWS keys want to be very careful with them, not on github or losing
* higher risk on watchers for AWS keys versus something like a Flicker key

* Start account with IAM
  * make a user
  * give it the permissions it needs
  (easier to start with broad permissions and change later)

* find the doc on the service and then make the request:
  * SDK or REST?
  * Tradeoffs: SDKs may be easier to start with but the actual APIs aren't that complicated to use directly SDKs maybe don't have the newest services or API versions
  * signature within signing key is one of the more complicated parts of making the request
    * cryptography if interested further in how it works
    * which one is the key and which is the signature you are encrypting


## Specific Services

* helps to understand how it compares through looking deeper on a service:

* EC2 - Elastic Computer Cloud
  * kind of like a VPS (VPS is what most relatively smaller "cloud" hosting things will give you, like Digital Ocean, Linode)
    * they make room on the virtual machine for your service based on your needs (renting a VPS)
    * now when you also need to increase your server size, you can reboot your VM as they shuffle the VM storage around to accomodate your new requirements
    * can use like a VPS, but not really that simple
    * seems to be more for companies now that need more servers but don't want to actually pay for them
    * or also when spikes are required by the service, spurts of time when you need more server drive than others
  * can use just for 3 hours a day when you need a lot of computing power but then remove it when no longer need (getting value of highend servers without paying and maintaining them)
* ELB - Elastic Load Balancer
  * Companies running all of their data on VPS machines would likely use this

* S3 - Scalable Storage Service
  * need to store a lot of data that eats up our collection server (avatars etc)
  * keep this info seperate so that you can download your main server content faster
  * permissions model on it - admins can only read, or no one can upload here
  * therefore the img is going through our server in order to upload to the S3
  * but of course thats not helping take the strain off the main server
  * you can give a one time ticket to the user (expires) in order for them to directly upload to the S3
  
**Further Customized Services**

* Lamda - Deploy function
  * microservices like
  * run a single function as an independent "server"
  * trigger it from S3 from S3 rules
  * have a bunch of tiny APIs rather than keep them all together
  * take a little bit of code, and that function becomes it own API when its needed
  * if you have your own server this wouln't be useful
  * tradeoffs: harder to debug, not your own server now, and then need Cloudwatch to actually start monitoring
  * when an object is created in the bucket, it triggers Lamda to also send the rendering of the video to a different bucket etc

* Elastic Video Transcode
  * a video transcoder that you feed configuration optons
  * useful because its a very CPU/GPU intensive process that your main application server probably doesn't really need the capacity for

* SQS - Simple Queing Service
  * first in first out line
  * concurrent processes
  * distrubuted service, therefore dealing with many communications and need to ensure you handle the queue a particular way
  * when x happens, new message adds to the queue to tell y with a message

(video rendered, and SQS sent message to our app server, and now we send a text to our user using SNS)

* SNS - Simple Notification Service
  * send text messages or push notifications to users

* SES - send email notification

**CDN**

* Cloudfront - content available quickly to users everywhere

* Rek - face recognition app

### questions

**Monitoring**

* what if using, how do we look at how to monitor those? (cloudwatch?)

* make jobs on it that send pings when you need them to - easier to build them into your own service

**Employer expectations**

* might be more curious about if you know what S3 does, of course depends on where going
* but less interested in certification as what you've build using it
* if needed to do a lot of scaling with data at a company, would be good to show that you understand AWS in order to exemplify understanding of working with multiple servers

