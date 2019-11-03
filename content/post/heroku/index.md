+++
title = "Deploying a Flask app to Heroku"

date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false

# Authors
authors = ["Michael W. Brady"]
+++
# Heroku
1. Create a new app through the Heroku web interface
2. Install Heroku command line tools (if not installed). Confirm install with `which heroku`
3. Log into Heroku on the command line with: `heroku login`
4. Ensure you are in correct directory and virtual environment
5. Establish a remote git connection with: `heroku git:remote -a your-app-name`
    1. Note: this step assumes a git repository has already been initialized (likely with GitHub)
6. Install gunicorn into the virtual environment: `pipenv install gunicorn`
    1. Gunicorn is an alternative to launching and running a server with `run flask` — the key difference is Gunicorn is production grade and scales well. Further, Gunicorn exposes far fewer debugging tools ... which is what you want in production! You can launch a local server with gunicorn with: `gunicorn your-app-name:APP`
7. Will also need to install psycopg2 (dependency for postgres database) to the virtual environment: `pipenv install psycopg2`
    1. Note: the latest version of psycopg has dependencies that may cause installation to fail. The workaround is to install 2.7 — `pipenv install psycopg2==2.7`
8. Create a Procfile — The procfile specifies the command that heroku will run to start the server. The procfile should be at the same level in the directory as the pipfile.
9. Push to Heroku: `git push heroku master` (it is also likely a good idea to push to github as well: `git push`
10. Add environment variables assignments: Easily done through the heroku web interface — found under application settings (config variables)
11. Create database for application with: `heroku addons:create heroku-postgresql:hobby-dev` 
    1. The default databse url will be `DATABASE_URL` if this matches what you set locally then it will work seamlessly

# Helpful commands

`heroku config` — view all environment variables

`heroku logs` — view logs

`pipenv update` — Update the virtual environment which updates the pipfile.lock if it is out of sync with the pipfile

`heroku run bash --app YOURAPPNAME` — Access dyno's command line (useful for investigating file structure)