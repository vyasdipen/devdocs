---
layout: default
group: install_pre
title: PHP
version: 2.3
github_link: install-gde/prereq/php-centos-ubuntu.md
functional_areas:
  - Install
  - System
  - Setup
---

This page details how to install PHP for both CentOS and Ubuntu systems.

<div class="bs-callout bs-callout-info" id="info" markdown="1">
Magento, with assistance from our community, is implementing PHP 7.2 compatibility for our upcoming 2.3.0 release. Any backward-incompatibility issues will be resolved in this release, and all 3rd party libraries now support PHP 7.2. Fully tested 7.2 support will be delivered in following patch releases.

If you are interested in participating in Magento Community projects we welcome your help! See our <a href="https://app.zenhub.com/workspace/o/magento-engcom/php-7.2-support/boards?repos=116423356,116426364,115111902" target="_blank">ZenHub board</a> for a full list of outstanding issues.
</div>

## Supported PHP versions {#php-support}
{% include install/php_2.3.md %}

<div class="bs-callout bs-callout-info" id="info" markdown="1">
If you must install both Apache and PHP, <a href="{{page.baseurl}}/install-gde/prereq/apache.html">install Apache</a> first.
</div>

## PHP for CentOS

The following sections give you all the info you need to install PHP for CentOS.

### Verify PHP is installed {#centos-verify-php}
To verify if PHP is installed already, enter `php -v`. If {% glossarytooltip bf703ab1-ca4b-48f9-b2b7-16a81fd46e02 %}PHP{% endglossarytooltip %} is installed, a message similar to the following displays:

    PHP 7.1.6 (cli) (built: Jan  9 2017 09:23:16) ( NTS )
    Copyright (c) 1997-2017 The PHP Group
    Zend Engine v3.1.0, Copyright (c) 1998-2017 Zend Technologies with Zend OPcache v7.1.6, Copyright (c) 1999-2017, by Zend Technologies

<div class="bs-callout bs-callout-info" id="info" markdown="1">
The preceding message confirms that the <code>Zend OPcache</code> is installed. We strongly recommend using the OPcache for performance reasons. If your PHP distribution does not come with the OPcache, see the [PHP OPcache documentation](http://php.net/manual/en/opcache.setup.php){:target="&#95;blank"}.
</div>

If PHP is installed, continue with the next prerequisite, [MySQL]({{page.baseurl}}/install-gde/prereq/mysql.html).

If PHP is *not* installed, see the [PHP 7.1 on CentOS](#php-centos-71) section.


### CentOS repositories {#centos-php-repos}
Linux systems provide software like PHP in one or more *repositories*. CentOS, unlike Ubuntu, has a set of [officially recommended repositories](https://wiki.centos.org/AdditionalResources/Repositories){:target="&#95;blank"}. Other repositories are considered less safe for the reasons stated on the CentOS wiki.

We're not aware that you can install PHP 7.1 or 7.2 from a CentOS-recommended repository. Therefore, you must consider the following:

*   If you're setting up a system that will be deployed in production, you should choose a hosting provider who uses repositories considered to be safe and reliable.

    You should also consider upgrading to a later version of CentOS that has the desired PHP version in a recommended repository.
*   If you're setting up a development system, you can use any repository you wish.

In this topic, we show how to install PHP using the [Inline with Upstream Stable (IUS)](https://ius.io/GettingStarted){:target="&#95;blank"} repository, which is *not* on the CentOS recommended list. However, packages installed from IUS do not use the same names as CentOS-provided packages, so [no existing system packages are replaced](https://ius.io/Philosophy){:target="&#95;blank"}.

Before you continue, review their [Getting Started topic](https://ius.io/GettingStarted){:target="&#95;blank"}.

<div class="bs-callout bs-callout-warning" markdown="1">
Magento does <em>not</em> officially recommend using the IUS repository. We discuss it here for example purposes only.
</div>

### PHP 7.1 on CentOS {#php-centos-71}
There is more than one way to install PHP 7.1 on CentOS; the following is a suggestion only. Consult a reference for additional options.

To install PHP 7.1 on CentOS 6 or 7:

1.	*CentOS 6*. Enter the following commands in the order shown:

		yum -y update
		yum -y install epel-release
		wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm
		wget https://centos6.iuscommunity.org/ius-release.rpm
		rpm -Uvh ius-release*.rpm
		yum -y update
	
	*CentOS 7*. Enter the following commands:

		yum install -y http://dl.iuscommunity.org/pub/ius/stable/CentOS/7/x86_64/ius-release-1.0-14.ius.centos7.noarch.rpm
		yum -y update

3.	Install all [required PHP extensions]({{page.baseurl}}/install-gde/system-requirements-tech.html#required-php-extensions):

		yum -y install php71u php71u-pdo php71u-mysqlnd php71u-opcache php71u-xml php71u-gd php71u-devel php71u-mysql php71u-intl php71u-mbstring php71u-bcmath php71u-json php71u-iconv php71u-soap

	<div class="bs-callout bs-callout-info" id="info">
  		<p>The <code>bcmath</code> extension is required for {{site.data.var.ee}} only.</p>
	</div>

4.	Restart Apache:
	
		service httpd restart`

5.	Verify that PHP 7.1 is installed properly:

		php -v

    The following response indicates that PHP 7.1.6 is installed:

		PHP 7.1.6 (cli) (built: Jan  9 2017 09:23:16) ( NTS )
		Copyright (c) 1997-2017 The PHP Group
		Zend Engine v3.1.0, Copyright (c) 1998-2017 Zend Technologies with Zend OPcache v7.1.6, Copyright (c) 1999-2017, by Zend Technologies

    <div class="bs-callout bs-callout-info" id="info" markdown="1">
    The preceding message confirms that the <code>Zend OPcache</code> is installed. We strongly recommend using the OPcache for performance reasons. If your PHP distribution does not come with the OPcache, see the  [PHP OPcache documentation](http://php.net/manual/en/opcache.setup.php){:target="&#95;blank"}.
    </div>

6.  Verify that all [required PHP extensions]({{ page.baseurl }}/install-gde/system-requirements-tech.html#required-php-extensions) were installed:

        php -me

    You should see output similar to the following:
    <pre>
    [PHP Modules]
    bcmath
    calendar
    Core
    ctype
    curl
    date
    dom
    exif
    fileinfo
    filter
    ftp
    gd
    gettext
    hash
    iconv
    intl
    json
    libxml
    mbstring
    mysqli
    mysqlnd
    openssl
    pcntl
    pcre
    PDO
    pdo_mysql
    Phar
    readline
    Reflection
    session
    SimpleXML
    soap
    sockets
    SPL
    standard
    tokenizer
    wddx
    xml
    xmlreader
    xmlwriter
    xsl
    Zend OPcache
    zip
    zlib

    [Zend Modules]
    Zend OPcache
    </pre>

7.	Continue with [Required PHP settings]({{ page.baseurl }}/install-gde/prereq/php-settings.html).


## PHP for Ubuntu

The following sections give you all the info you need to install PHP for Ubuntu.

### Verify PHP is installed {#ubuntu-verify-php}
To verify if PHP is installed already, enter `php -v`. If PHP is installed, a message similar to the following displays:

    PHP 7.1.7-1~ubuntu14.04.1+deb.sury.org+1 (cli) (built: Jul  6 2017 09:07:54) ( NTS )
    Copyright (c) 1997-2017 The PHP Group
    Zend Engine v3.1.0, Copyright (c) 1998-2017 Zend Technologies with Zend OPcache v7.1.7-1~ubuntu14.04.1+deb.sury.org+1, Copyright (c) 1999-2017, by Zend Technologies

<div class="bs-callout bs-callout-info" id="info" markdown="1">
The preceding message confirms that the <code>Zend OPcache</code> is installed. We strongly recommend using the OPcache for performance reasons. If your PHP distribution does not come with OPcache, see the [PHP OPcache documentation](http://php.net/manual/en/opcache.setup.php){:target="&#95;blank"}
</div>

If PHP is installed, continue with the next prerequisite, [MySQL]({{page.baseurl}}/install-gde/prereq/mysql.html).

If PHP is *not* installed see the next section, PHP 7.1 on Ubuntu.

### PHP 7.1 on Ubuntu {#instgde-prereq-php71-ubuntu}

To install PHP 7.1 on Ubuntu 14 or 16:

1.	Enter the following commands in the order shown:

		sudo apt-get -y update
		sudo add-apt-repository ppa:ondrej/php
		sudo apt-get -y update
		sudo apt-get install -y php7.1 libapache2-mod-php7.1 php7.1-common php7.1-gd php7.1-mysql php7.1-curl php7.1-intl php7.1-xsl php7.1-mbstring php7.1-zip php7.1-bcmath php7.1-iconv php7.1-soap

    <div class="bs-callout bs-callout-info" id="info" markdown="1">
    The last command installs all [required PHP extensions]({{page.baseurl}}/install-gde/system-requirements-tech.html#required-php-extensions). The <code>bcmath</code> extension is required for {{site.data.var.ee}} only.
    </div>

2.	Verify that PHP 7.1 is installed properly:

		php -v

    The following response indicates that PHP 7.1.7 is installed:

		PHP 7.1.7-1~ubuntu14.04.1+deb.sury.org+1 (cli) (built: Jul  6 2017 09:07:54) ( NTS )
		Copyright (c) 1997-2017 The PHP Group
		Zend Engine v3.1.0, Copyright (c) 1998-2017 Zend Technologies with Zend OPcache v7.1.7-1~ubuntu14.04.1+deb.sury.org+1, Copyright (c) 1999-2017, by Zend Technologies

    <div class="bs-callout bs-callout-info" id="info" markdown="1">
    The preceding message confirms that the <code>Zend OPcache</code> is installed. We strongly recommend using the OPcache for performance reasons. If your PHP distribution does not come with the OPcache, see the [PHP OPcache documentation](http://php.net/manual/en/opcache.setup.php){:target="&#95;blank"}.
    </div>

3.	Verify that all [required PHP extensions]({{ page.baseurl }}/install-gde/system-requirements-tech.html#required-php-extensions) were installed:

        php -me

    You should see output similar to the following:
    <pre>
    [PHP Modules]
    bcmath
    calendar
    Core
    ctype
    curl
    date
    dom
    exif
    fileinfo
    filter
    ftp
    gd
    gettext
    hash
    iconv
    intl
    json
    libxml
    mbstring
    mysqli
    mysqlnd
    openssl
    pcntl
    pcre
    PDO
    pdo_mysql
    Phar
    readline
    Reflection
    session
    SimpleXML
    soap
    sockets
    SPL
    standard
    tokenizer
    wddx
    xml
    xmlreader
    xmlwriter
    xsl
    Zend OPcache
    zip
    zlib

    [Zend Modules]
    Zend OPcache
    </pre>

### Install optional `mcrypt`

#### For PHP 7.1

`mcrypt` is included in PHP 7.1 and should be installed from the native `mcrypt` library.

#### For PHP 7.2

`mcrypt` is not included in PHP 7.2 and is not required for Magento v2.3. To improve encryption performance, however, we suggest installing `mcrypt` from the [PHP Extension Community Library](https://pecl.php.net/) (PECL).

To install `mcrypt`:
1. Check the PHP and PECL versions:
        
        php -v
        
        pecl version

1. Install the `mcrypt` extension:
        
        sudo apt-get -y install gcc make autoconf libc-dev pkg-config
        
        sudo apt-get -y install libmcrypt-dev
        
        sudo pecl install mcrypt-1.0.1

    If you get a `PHP Fatal error:  Cannot use result of built-in function in write context in /usr/share/php/Archive/Tar.php` error, install PEAR and then retry the prior step:

        sudo apt-get install php-pear

1. When you are shown the autodetect prompt press Enter.
2. Add the install path to both `/cli` and `/apache2` directories:
        
        sudo bash -c "echo /your/install/path/mcrypt.so > /etc/php/7.2/cli/conf.d/mcrypt.ini"
        
        sudo bash -c "echo /your/install/path/mcrypt.so > /etc/php/7.2/apache2/conf.d/mcrypt.ini"

1. Verify that the extension was successfully installed:
        
        php -i | grep mcrypt

The output will look like this:
         
         /etc/php/7.2/cli/conf.d/mcrypt.ini
        Registered Stream Filters => zlib.*, string.rot13, string.toupper, string.tolower, string.strip_tags, convert.*, consumed, dechunk, convert.iconv.*, mcrypt.*, mdecrypt.*
        mcrypt
        mcrypt support => enabled
        mcrypt_filter support => enabled
        mcrypt.algorithms_dir => no value => no value
        mcrypt.modes_dir => no value => no value

#### Next
[Required PHP settings]({{ page.baseurl }}/install-gde/prereq/php-settings.html)
