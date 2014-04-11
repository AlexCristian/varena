mule
====

Varena is a website program for hosting programming problems. It is based on [mule](https://github.com/CatalinFrancu/mule), a very lightweight framework.

Installation and contributions
------------------------------

For simplicity, we will not follow the normal development process (fork -> modify -> commit -> submit a pull request). Instead, we will add contributors to the organization so they can work on this repository. If you are familiar with forks and pull requests, you are welcome to do that too.

* Get a copy of the code:

        $ git clone https://github.com/varena/varena

* Some light local configuration:

        $ cd varena
        $ tools/setup.sh

* Create the database (adapt to suit your needs):

        $ mysql -u root -e 'create database varena charset utf8'

* Modify mule.conf to reflect your locale, database config etc.
* Apply any patches to bring the database schema to date:

        $ php tools/migration.php

* Depending on your HTTP server and installation path, you may need to enable and configure various modules: userdir, rewrite, PHP. Suggested changes (for Ubuntu / Linux Mint, everything as sudo):
  * Install mod-php5.

          # apt-get install libapache2-mod-php5

  * Enable the rewrite module. This is only used to hide .php extensions in URLs

          # a2enmod rewrite

  * Enable the userdir module if you're working under ~/public_html/

          # a2enmod userdir

  * Modify the relevant config file to allow .htaccess files:

          # for userdir installations the file is usually /etc/apache2/mods-available/userdir.conf
          <Directory /home/*/public_html>
              ...
              AllowOverride All
              ...
          </Directory>
          
          # for document root installations the file is usually /etc/apache2/sites-available/default
          <Directory /var/www/>
              ...
              AllowOverride All
              ....
          </Directory>

  * Restart Apache:

          # /etc/init.d/apache2 restart


Localization (for users)
------------------------

To use Varena in your language, you need three things:

* Install the locale you want on your system
* Make sure there is a corresponding entry in the locale/ directory. Note that the names must match exactly. For instance, if your system has a ro_RO locale, but the locale/ directory has a ro_RO.utf8 subdirectory, localization won't work.
* Someone actually needs to do the translation work for your language. You can be that person! :-) See below for contributions.


Localization (for programmers)
-------------------------------


We'd like to keep up with localization. You don't need to do the translation yourself, but we'd appreciate it if you tagged the strings that need be localized.

* In PHP, just add a call to gettext() around all literal strings. So instead of 

        return 'OpenID login failed';

  please say

        return _('OpenID login failed');

* In Smarty templates, use the "_" variable modifier around string literals. We recommend doing this at paragraph level:

        {"String to be localized"|_}

You can use sprintf in both PHP and Smarty so you can localize parametric sentences:

        {"User %s obtained %d points."|_|sprintf:"John":"100"}

Note that this syntax does not allow for changes in argument orders between languages. We'll try to live with it.

Localization (for translators)
------------------------------

We use poedit for translation. It needs some light customization for the Smarty syntax (poedit doesn't know it by default).

* Install poedit;
* Run

        poedit locale/ro_RO.utf8/LC_MESSAGES/messages.po

* Go to Edit -> Preferences -> Personalize and enter your name and email address;
* Under Edit -> Preferences -> Parsers, hit New to add a new Parser. Set these values:
  * language: Smarty
  * extensions = *.tpl
  * parser command = php /path/to/tplParser.php %o %F
  * an item in input files list = %f

To do the actual translation:

* Run Catalog -> Update from sources;
* Translate any new strings.
