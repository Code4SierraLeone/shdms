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

### Heroku

...

