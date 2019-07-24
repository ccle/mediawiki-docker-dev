# Mediawiki Environment Setup (CCLE Developers)

```
cd ~/Projects
git clone git@github.com:ccle/mediawiki-docker-dev.git mediawiki
cd mediawiki
git clone git@github.com:ucla/ccledocs.git ccledocs
cd ccledocs
git checkout master131
git pull
```

```
cd ~/Projects/mediawiki/ccledocs
cp ../config/mediawiki/LocalSettings.php LocalSettingsPrivate.php
ln -s LocalSettingsDev.php LocalSettings.php
cp 
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

 - [Default MediaWiki Site](http://default.web.mw.localhost:8080)
* [PhpMyAdmin](http://phpmyadmin.mw.localhost:8080)
  * username: root
  * password: toor

