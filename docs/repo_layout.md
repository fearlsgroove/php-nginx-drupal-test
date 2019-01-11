## Repository Layout Guidelines

We recommend laying out your source git repository in the following way, to enable you to use all the features of the container.

It's important to note code will always be checked out to ```/srv/www/``` this is for historic reasons and we may improve this in the future with a user configurable variable. If you just wish to check code out into a container and not do anything special simply put all your files in the root directory of your repository like so:

```
- repo root (/srv/www)
 - index.html
 - more code here
```

However if you wish to use scripting support you'll want to split code and scripts up to ensure your scripts are not in the public part of your site.

```
 - repo root (/srv/www)
  - src
    - your code here
  - conf
    - nginx
      - nginx-site.conf
      - nginx-site-ssl.conf
  - scripts
    - 00-firstscript.sh
    - 01-second.sh
    - ......
```

### src / Webroot
If you use an alternative directory for your application root like the previous example of __src/__, you can use the __WEBROOT__ variable to instruct nginx that that is where the code should be served from.

``` docker run -e 'WEBROOT=/srv/www/src/' -e OTHER_VARS ........ ```

One example would be, if you are running craft CMS you'll end up with a repo structure like this:

```
- repo root (/srv/www)
 - craft
   - core craft
 - public
   - index.php
   -    other public files
```

In this case __WEBROOT__ would be set as __/srv/www/public__

Note that if you are managing dependencies with composer, your composer.json and composer.lock files should *always* be located in the repo root, not in the directory you set as __WEBROOT__.

### conf
This directory is where you can put config files you call from your scripts. It is also home to the nginx folder where you can include custom nginx config files.

### scripts
Scripts are executed in order so its worth numbering them ```00,01,..,99``` to control their run order. Bash scripts are supported but, of course, you could install other run times in the first script then write your scripts in your preferred language.
