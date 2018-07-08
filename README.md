# Migration of demo.mirawatt.com from Axial servers
On 2018-07-05 I migrated all the data on axial servers
which histed multiple sites. One of the sites (demo.mirawatt.com) also had a database, being a wordpress site.

## TODO
- [Publish from master/docs](https://help.github.com/articles/configuring-a-publishing-source-for-github-pages/)
- [Simply Static (export)](https://wordpress.org/plugins/simply-static/)
- [Static Html Ouput](https://wordpress.org/plugins/static-html-output-plugin/)
- [WP to hugo](https://github.com/SchumacherFM/wordpress-to-hugo-exporter)
- iMetrical: <img src="https://latex.codecogs.com/svg.latex?%5Clarge%20%5Cparallel%7Bi%7D%5Cparallel" title="\parallel{i}\parallel" />

## iMetrical Latex logo
https://www.mathjax.org/#demo
```
$${\parallel{i}\parallel}^2 = \langle i,i \rangle $$
$${\parallel{i}\parallel} = {\langle i,i \rangle}^{.5} $$
$${\parallel{i}\parallel} = {\langle i,i \rangle}^{1\over{2}} $$
$${\parallel{i}\parallel} = \sqrt{\langle i,i \rangle } $$
$${\parallel{i}\parallel} = \sqrt{\smash[b]{\langle i,i \rangle }} $$
```

## History
 Also see `/archive/mirror/mirawatt/README.md`

### Export database (demo.mirawatt.com)
-Used PHPMyAdmin to dump Mysql Database: mirawatt_wordpress (556KB)

### Migrate Filesystem/WebSites
Used FileZZilla to transfer
- from ftp://mirawatt.com@mirawatt.cld-linux01.axialdev.net/  
- to `/archive/mirror/mirawatt/Axial-All-httpdocs-2018-07-05.tgz`

### Version of php, etc...
Copied `info.php` to `mirawatt.cld-linux01.axialdev.net:/httpdocs/demo`
  - PHP Version 5.3.2-1ubuntu4.30
  - http://demo.mirawatt.com/info.php
  - full output saved here: './phpinfo-capture-2018-07-05.html`

## Running locally
```
# create database
docker-compose build
docker-compose up -d
# restore database
docker-compose exec mysql bash -c "mysql mirawatt_wordpress </backup/mirawatt_wordpress-2018-07-05-edited.sql"

# will cause DB to be upgraded
open http://localhost/wp-admin/
```

## Export by hand
Since I only have a few pages, start with ted.mirawatt.com (in `./docs`), and move content manually

## Static export
Could not make this work
### Static export with static-html-output-plugin.4.0
Neede to tell it where the new site will be hosted (http://demo.mirawatt.com).
-index.html: <img style="margin-top:-15px" src="contents/ui/theme/images/mirawatt-wp-pixel-logo-400x75-trans.png" >
### Static export with simply-static.2.1.0
Causes an error on export: can't find error

## Converting to hugo:
```
docker-compose exec wordpress bash

cd /var/www/html/wp-content/plugins/
apt update && apt install -y git
git clone https://github.com/SchumacherFM/wordpress-to-hugo-exporter.git
cd wordpress-to-hugo-exporter/
php hugo-export-cli.php

# then from outside:
docker cp demomirawattcom_wordpress_1:/tmp/wp-hugo.zip .
```

### Extra notes
```
# escalate privileges - not necessary for wordpress ??
GRANT ALL PRIVILEGES ON *.* TO 'mirawatt.co_user'@'%';FLUSH PRIVILEGES;

# from outside
docker run --rm -it mysql:5 mysql -h dirac.imetrical.com mirawatt_wordpress -e 'show tables'
# from outside as user mirawatt.co_user ??? does not work
docker run --rm -it mysql:5 mysql -h dirac.imetrical.com mirawatt_wordpress -u mirawatt.co_user -p=hu73cF -e 'show tables'
```

## Version of php, etc...
Copied `info.php` to `mirawatt.cld-linux01.axialdev.net:/httpdocs/demo`
  - PHP Version 5.3.2-1ubuntu4.30
  - http://demo.mirawatt.com/info.php
  - full output saved here: './phpinfo-capture-2018-07-05.html`

```
PHP Version 5.3.2-1ubuntu4.30
```
