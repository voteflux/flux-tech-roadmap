# Membership UI

Some ideas around how to handle the Membership UI v2 (which is something we should do before working on the voting system). 

## Backend - Prelim

So if we're updating the membership UI we should consider updating the backend too. 
There's a lot with the backend I (Max) am not happy with. To date I'm the only contributor to the current backend.

* Closed source (basically bc it's been hacked together and I don't want to expose any vulnerabilities that might leak member data)
* One big python app - not modular, I don't use python much anymore, and harder to incrementally update
* Heroku - basically a cost thing ($50/mo) - we need at least 2 dynos (one admin / scheduler, one web handler), and each additional dyno costs another $25
* MongoDB hosted with MLab - another $18/mo - we could use AWS DynamoDB to reduce costs quite a bit

## Membership UI v2 Ideas

* Use an SPA style architecture that can be compiled to an app (via e.g. cordova) / be a progressive web app
* Use common frameworks (Note to Max: no Elm/Purescript)
* Maybe Vue.js?
* Re implement _all_ features to give us a much nicer framework and better consistency
* Implement a nicer admin util (current admin page can be seen (without logging in) at https://api.voteflux.org/static/html/admin.html)
* Design it as modularly as possible
* Use ES7 syntax + Build tool (e.g. webpack) to transpile to ES5 or whatever
* Run dev and project via github repo
* Open source from the start

Note: I actually started on a membership v2 page in Elm (https://voteflux.org/members/) that doesn't do much but does authenticate / login, etc (I think)

## Backend - Ideas

**Ideas for making the backend better**

* Use AWS lambda
  * Makes it a bunch more modular (individual handlers can be written in NodeJS, Python, Go, Java, and C#)
  * Easier for ppl to do drive-by contribution
  * Super cheap (free up to 1m requests per month)
  * Cron functionality is now a super small cost (basically we have cron-job-type-things that need to be run once every x seconds (x in range [10, 86400]) - Lambda allows scheduling this sort of thing
  * Can implement CI / CD
* **New authentication**
  * Probs first thing on new backend we need to do
  * Authentication atm is just using a static token - this is Not Good (tm)
  * Better: use JWT tokens that are issued to users (can set a long expiry, e.g. 3 months)
  * Use email to authenticate users still (avoids usernames/passwords - most ppl are really bad at this)
* Allows us to use AWS's email functionality which is much cheaper than MailChimp (both for lists and for transactional)
* Use templates to generate emails for lists / auto emails (currently out transactional emails are all plaintext)
* Can implement backwards compatibility with existing DB while we implement V2
* Fully document API (I have some docs for API v1 somewhere, but can't find it atm)
* Open source from start
* Fine grain permissions around everything - e.g. organisers for VIC can freely email VIC members, but not anyone else (currently not possible with MailChimp)
