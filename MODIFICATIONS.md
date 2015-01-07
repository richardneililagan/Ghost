# Modifications from the core Ghost platform

This file will list all modifications / changes made off the core behavior of the
[Ghost](https://github.com/TryGhost/Ghost) blogging platform.

> This fork is currently based off of [v0.5.7](This fork of the Ghost platform is currently based off of v0.5.7).

## Shortlist

  * Prepare the release package.
  * Heroku deployment support


## Prepare release package.

[Grab a release package from Github](https://github.com/richardneililagan/Ghost/releases/).

Releases under the modified Ghost platform will be in the format `vX.X.X.y-y.RNAI`,
where `X.X.X` signifies the base Ghost version the release was based off of,
and `y-y` specifies the versioning for the particular package.

Generally, the latest `RNAI` release should be good to go.

Unzip the contents of the package into the directory of your choice.

Open `config.js` and modify `config.production.url` (line 13) to match the production URL of your site.


## Heroku deployment support

This version of Ghost should be deployable (and usable) in a Heroku environment out of the box.
The main changes required are that:

  1. The underlying database used is now Postgres (from SQLite).
  2. Emails are routed through Mandrill, through the Heroku addon module.

Before deploying the Ghost platform to your Heroku instance, we'll need to prepare Heroku first.

You'll need the [Heroku toolbelt](https://toolbelt.heroku.com/) to do the following from the command line.
You can opt to do the following steps from the Heroku site itself, but the steps described below
are meant to be performed from the command line.

Create a new Heroku app instance. Rename `myGhost` to the name of your app.
This will also add the newly created Heroku app as a remote to your Ghost clone.

    heroku apps:create myGhost


### Prepare Postgres database

Then we add on Postgres into our Heroku instance.
Ghost will use this instead of SQLite.

__NOTE :__ Change `hobby-dev` to the Postgres tier you want to use with your app.
[Have a gander at the detailed listing of Heroku Postgres tiers and pricing here](https://addons.heroku.com/heroku-postgresql).

    heroku addons:add heroku-postgresql:hobby-dev


### Prepare email service (via Mandrill)

This Ghost installation will use Mandrill on Heroku to send emails when it needs to.

__NOTE :__ Change `starter` to the Mandrill tier you want to use with your app.
[Check out the Mandrill tiers on Heroku here](https://addons.heroku.com/mandrill).

    heroku addons:add mandrill:starter

### Do the deploy

It's as easy as:

    git push heroku master

This will initialize your Ghost install on Heroku, but by default, it will be running in __development__ mode.
To switch it to __production__, run the following:

    heroku config:set NODE_ENV=production

