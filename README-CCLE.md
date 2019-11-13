# Mediawiki Environment Setup (CCLE Developers)

```
cd ~/Projects
git clone git@github.com:ccle/mediawiki-docker-dev.git mediawiki
cd mediawiki
git clone git@github.com:ucla/ccledocs.git ccledocs
cd ccledocs
git checkout master131
git pull
git submodule update --init --recursive
```

```
cd ~/Projects/mediawiki/ccledocs
cp ../config/mediawiki/LocalSettings.php LocalSettingsPrivate.php
ln -s LocalSettingsDev.php LocalSettings.php
```

```
cd ~/Projects/mediawiki
cp default.env local.env
cd ccledocs
pwd
```
copy the path\
change line 40 of local.env. Assign **DOCKER_MW_PATH** to the absolute path of ccledocs dir.
```
cd ~/Projects/mediawiki/ccledocs
sudo docker run -it --rm --user $(id -u):$(id -g) -v ~/.composer:/tmp -v $(pwd):/app docker.io/composer install
```

Add this to a new line in your **~/.bashrc** file (Ubuntu) **~/.bash_profile** (Mac)\
Don't forget to replace \<absolutepath to mediawiki repo\>
```
alias mw-docker-dev='_(){ (cd <absolutepath to mediawiki repo>; sudo ./$@) ;}; _'
```
Open a new terminal\
refer [here](https://github.com/ccle/mediawiki-docker-dev#create) to start, suspend, destroy etc.

```
mw-docker-dev create
mw-docker-dev hosts-sync
```
Open http://default.web.mw.localhost:8080/ in your browser

## Access

* [Default MediaWiki Site](http://default.web.mw.localhost:8080)
 * username: Admin
 * password: dockerpass
* [PhpMyAdmin](http://phpmyadmin.mw.localhost:8080)
  * username: root
  * password: toor

 ## Probable Setup issues
 
 1. If there is a warning sign of multiple undefined index variables for SpecialStatusPage.php
   - Import the SQL files to seed the status page with categories and items:
      -status_page_categories.sql
      -status_page_items.sql
   - These files are available in jira https://ucla-ccle.atlassian.net/browse/CCLE-8525
 
 2. If you see a warning (Incorrect format parameter) while importing Database dump mediawiki.2019-04-01_0315.gz on phpMyAdmin
   - Solution 1: 
  ``` yaml
   Go to docker using ./bash command and update the php.ini file
      - upload_max_filesize=128M
      - post_max_size=128M
  ```  
   - Solution 2: 
   ``` yaml
      If the Solution 1 above doesn't help, do the import manually
      a. Go to mediawiki folder (One folder down from ccledocs)
      b. Find the databsae server name using docker ps -a
      c. Run docker-compose exec "db-master" sh -c "bash" (Where db-master is the database server name)
      d. Type Exit and press Enter (Steps b,c and d are to confirm that you have the right database server name and it's                access)
      e. Now copy the import file from your local computer to database server.
         - wget http://test.ccle.ucla.edu/vagrant/mediawiki.2019-04-01_0315.gz to downloads folder and unzip it
         - docker cp /Users/<username>/Downloads/<mediawiki.2019-04-01_0315> mediawiki_db-master_1:/tmp/mediawiki.2019-04-                01_0315
         - Now that you copied the file to the db container, Run docker-compose exec "db-master" sh -c "bash"
         - Go to /tmp make sure the file is copied
         - Stay in /tmp
         - mysql -u root -p
         - Password: toor
         - use default;
         - source mediawiki.2019-04-01_0315;
         - Import should be done in no time.
         - Exit form database
         - Exit from database server
         - Run ./bash in mediawiki folder
         - Run maintenance/update.php
   ```
