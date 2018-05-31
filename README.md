# Salone HDMS

Salone HDMS is an Emergency Development Environment - an Open Source framework to rapidly build powerful applications for Emergency Management.

It is a web based collaboration tool that addresses the common coordination problems during a disaster from finding missing people, managing aid, managing volunteers, tracking camps effectively between Government groups, the civil society (NGOs) and the victims themselves.

## Installation
This package is designed to be uploaded into a running instance of [Web2Py](https://github.com/web2py/web2py/)

### Local Installation
It's probable that you already have Python 2.7.x installed but check with python -V. If you don't, or want an OS-version-independent installation.

Install the following required Python packages:
```
pip install python-dateutil
pip install lxml
pip install shapely
pip install reportlab
pip install xlrd
pip install xlwt
pip install pyserial
pip install tweepy
pip install Pillow
```
For some GIS features of Salone HDMS (e.g. if you would rather use PostgreSQL with PostGIS instead of MySQL) you are required to install the  GDAL framework.  GEOS is also required, and is a dependency of shapely.

If you want to use PostgreSQL - PostGIS (for which you will need GDAL, above):

#### Install Web2Py
```
git clone https://github.com/web2py/web2py.git --recursive
```
Salone HDMS now supports both `web2py-2.14.6 (#cda35fd`) and `web2py-2.16.1 (#7035398)` stable releases, so you can either:

```
cd web2py
# Reset to web2py-2.14.6
git reset --hard cda35fd
git submodule update
```
or (recommended):
```
cd web2py
# Reset to web2py-2.16.1
git reset --hard 7035398
git submodule update
```

#### Install Salone HDMS
If you are deploying Salone HDMS code base but do not intend on contributing any changes back to the core trunk (perhaps for a custom deployment) you can just clone the Trunk version of Salone HDMS:

```
cd web2py/applications
git clone https://github.com/Code4SierraLeone/shdms.git
```
#### For Ongoing Development
If you intend to develop for Salone HDMS and have your code pulled into Trunk you need to follow the instructions Fork Your Own Salone HDMS Repository to be able to pull new updates and push changes.

To test your installation of Salone HDMS, without Eclipse, you can start web2py from the command line:

```
cd .. # the web2py main directory
python web2py.py
```

When you first start Salone HDMS, then the settings file web2py/application/shdms/models/000_config.py will be created from web2py/application/shdms/modules/templates/000_config.py. You will be unable to proceed until you edit this file. The minimal required change is to delete the line with FINISHED_EDITING_CONFIG_FILE = False.

Run this command to make the edit ☝️
```
# Copy and edit the config file
cp shdms/modules/templates/000_config.py shdms/models/000_config.py
cat shdms/models/000_config.py | sed "s/FINISHED_EDITING_CONFIG_FILE = False/FINISHED_EDITING_CONFIG_FILE = True/" > temp
mv temp shdms/models/000_config.py
```

### TEST Salone HDMS
Once you have web2py running navigate to the server (  http://localhost:8000 by default ). From the web2py drop down menu navigate to "My Sites" and Select Salone HDMS. You should now be at the Salone HDMS homepage.

By default the 1st user to register will gain the Administrator role.


## Heroku Deployment

THERE ARE STILL ISSUES WITH DEPLOYING Salone HDMS TO HEROKU - Please help us to get this working & improve these docs!

Deployment
The Heroku deployment process is very straightforward.:

1. Create Heroku Account
2. Install  Heroku Toolbelt
3. Create Git Branch with Web2Py + Salone HDMS
4. Create Heroku App

#### Create Git Branch With Web2Py + Salone HDMS
It's recommended to do this in a separate local directory to avoid interfering with your working Web2Py + Salone HDMS branches.
```
# Choose the Admin Password
read -p "Choose your admin password?" passwd

# Get latest web2py
git clone https://github.com/web2py/web2py.git web2py


# Remember to switch to the supported releases.

cd web2py
# Reset to web2py-2.14.6
git reset --hard cda35fd
git submodule update

# or (recommended):

cd web2py
# Reset to web2py-2.16.1
git reset --hard 7035398
git submodule update

cd web2py/applications

# Get Salone HDMS TRUNK.
git clone https://github.com/Code4SierraLeone/shdms.git shdms

# OR get your Salone HDMS branch.
git clone https://github.com/YOUR_GIT_USERNAME/shdms.git shdms

# Copy and edit the config file
cp shdms/modules/templates/000_config.py shdms/models/000_config.py
cat shdms/models/000_config.py | sed "s/FINISHED_EDITING_CONFIG_FILE = False/FINISHED_EDITING_CONFIG_FILE = True/" > temp
mv temp shdms/models/000_config.py
```

#### Create Heroku App
```
# Install virtualenv and postgres DB
sudo pip install virtualenv
sudo pip install psycopg2

# Activate the virtual environment
virtualenv venv --distribute
source venv/bin/activate

# Generate the requirements for Salone HDMS.
cp applications/shdms/requirements.txt .
echo "" >> requirements.txt
pip freeze >> requirements.txt

# Write the Procfile used by heroku
echo "web: python web2py.py -a '$passwd' -i 0.0.0.0 -p \$PORT" > Procfile

# Create a remote for heroku
heroku create

# Choose the application name
read -p "Choose your application name?" appname
heroku apps:rename $appname    

# Remove shdms from version control and add it to web2py version control (there should be one version control)
cd applications/shdms/
rm -rf .git
git add -f .
cd ../../

# When deploying a Python app on Heroku, it defaults to Python v3.x
# Since Salone HDMS works with Python v2.7.1x you'll need to create  a runtime.txt file to specify the python version

vi runtime.txt
# and paste this line
python-2.7.15

# Open .gitignore file and delete/comment line 18. This allows you to set the routes.py file. This files allows you to set the default application on web2py.
# Then create a new routes.py file 
vi routes.py

# and paste this line, 
routers = dict( 
    BASE = dict( 
        default_application='shdms',
    ) 
) 

git add .
git add Procfile
git commit -a -m "first commit"



# Push Salone HDMS to heroku
git push heroku master

# Add add-ons : Postgres DB
heroku addons:add heroku-postgresql:hobby-dev --app $appname
heroku scale web=1 --app $appname

# Open the application.
heroku open --app $appname
```

#### UPDATES
When changes merged and the pushed to Heroku the application is rebuilt with these changes.

Currently updates can only be made directly to the combined branch. We need instructions to set up the git branch so that the you can still update from the Salone HDMS branch, eg:

```
# THIS DOES NOT CURRENTLY WORK
# Pull changes from trunk
git pull upstream https://github.com/Code4SierraLeone/shdms.git master

# OR Pull changes from your branch 
git pull git://github.com/YOUR_GIT_USERNAME/shdms.git master

# Push Changes to Heroku App
git push heroku master
```

