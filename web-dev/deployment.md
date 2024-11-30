# Deployment of web apps

## Databases
1. For MongoDB databases, _MongoDB Atlas_ (paid cloud service) is an option
    - there is a free tier for small apps
    - can create database users with GUI
    - can edit permissions with GUI

## Deploying the actual app
For Express apps, you can use a few similar hosting tools:

1. Heroku.com (no free tier)
    - make an account
    - install the Heroku CLI
    - from the terminal, `heroku login`
    - `heroku create` makes a new application (gives you a new URL for your app)
    - `git add` stuff, then `git push heroku master` ('heroku' is the name of the remote that Heroku creates for you)
    - in `package.json`, we can create a `start` field in the `scripts` field, and give it a value that is equal to the command that should be used to start the app, e.g.
        ```
        "scripts": {
            "test": "something...",
            "start": "node app.js"
        }
        ```
        for the record, this also allows the use of `npm start` and that will also start the app
    - default port for Heroku is `80`, so the Express app should serve on that port
    - Configure Heroku ENVIRONMENT variables
        1. Dashboard > Settings (to add them manually)
        2. or from command line `heroku config:set VAR_NAME=fhaiwuehfieuahf` 
2. Render.com