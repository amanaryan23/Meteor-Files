[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/VeliovGroup/Meteor-Files-Demo)

Demo app
======
__Links:__
 - __[Heroku hosted Live Demo](https://meteor-files.herokuapp.com/)__

__Functionality:__
 - Upload / Download Files
 - Stream Audio / Video Files
 - Drag'n'drop support (*files only, folders is not supported yet*)
 - Image processing (*thumbnails, preview*)
 - DropBox as storage

Activate DropBox
======
 1. Read [this article](https://github.com/VeliovGroup/Meteor-Files/wiki/Third-party-storage)
 2. Set DropBox credentials into `METEOR_SETTINGS` env.var or pass as file, read [here for more info](http://docs.meteor.com/#/full/meteor_settings), alternatively (*if something not working*) set `DROPBOX` env.var
 3. You can pass DropBox credentials as JSON when using "*Heroku's one click install-button*"

DropBox credentials format:
```json
{
  "dropbox": {
    "key": "xxx",
    "secret": "xxx",
    "token": "xxx"
  }
}
```

Deploy to Heroku
======
 - Due to "*ephemeral filesystem*" on Heroku, we suggest to use DropBox as permanent storage, [read DropBox tutorial](https://github.com/VeliovGroup/Meteor-Files/wiki/Third-party-storage)
 - Go to [Heroku](https://signup.heroku.com/dc) create and confirm your new account
 - Go though [Node.js Tutorial](https://devcenter.heroku.com/articles/getting-started-with-nodejs)
 - Install [Heroku Toolbet](https://devcenter.heroku.com/articles/getting-started-with-nodejs#set-up)
 - Then go to Terminal into Meteor's project directory and run:

```shell
# Available architectures:
# os.osx.x86_64
# os.linux.x86_64
# os.linux.x86_32
# os.windows.x86_32
meteor build ../build-<your-app-name> --architecture os.linux.x86_64
cd ../build-<your-app-name>
tar xzf <name-of-archive> -C ./
cd bundle/
cp -Rf * ../
cd ../
rm -Rf bundle/
rm -Rf <name-of-archive>
touch Procfile
echo "web: node main.js" > Procfile

heroku create <your-app-name> --buildpack https://github.com/heroku/heroku-buildpack-nodejs
# This command will output something like: 
# - https://<your-app-name>.herokuapp.com/
# - https://git.heroku.com/<your-app-name>.git
heroku git:remote -a <your-app-name>

# Copy this: `https://<your-app-name>.herokuapp.com`, note `http(s)://` protocol
heroku config:set ROOT_URL=https://<your-app-name>.herokuapp.com
# To have a MongoDB, you can create one at https://mlab.com/
# After creating MongoDB instance create user, then copy URL to your MongoDB
# Should be something like: mongodb://<dbuser>:<dbpassword>@dt754268.mlab.com:19470/mydb
heroku config:set MONGO_URL=mongodb://<dbuser>:<dbpassword>@dt754268.mlab.com:19470/mydb

# For DropBox:
# heroku config:set DROPBOX='{"dropbox":{"key": "xxx", "secret": "xxx", "token": "xxx"}}'

# Enable sticky sessions, to support HTTP upload:
heroku features:enable http-session-affinity

git init
git add .
git commit -m "initial"
git push heroku master
```
 - Go to `https://<your-app-name>.herokuapp.com`
 - If your app has errors:
   * Check logs: `heroku logs --tail`
   * Try to run locally and debug: `heroku run node`