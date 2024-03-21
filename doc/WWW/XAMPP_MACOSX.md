
apache

apachectl start

Step 1 — Install or Restart Apache Web Sharing on Mac
To start Apache web sharing:

sudo apachectl start
To stop the Apache service:

sudo apachectl stop
To restart the Apache service:

sudo apachectl restart
To find the Apache version:

httpd -v
Now open your browser and open localhost — you will get this screen as below:





PHP


brew install php





To enable PHP in Apache add the following to httpd.conf and restart Apache:
cd /etc/apache2

sudo vi httpd.conf

LoadModule php_module /usr/local/opt/php/lib/httpd/modules/libphp.so

    <FilesMatch \.php$>
        SetHandler application/x-httpd-php
    </FilesMatch>

Finally, check DirectoryIndex includes index.php
DirectoryIndex index.php index.html

The php.ini and php-fpm.ini file can be found in:
/usr/local/etc/php/8.2/

To restart php after an upgrade:
brew services restart php
Or, if you don't want/need a background service you can just run:
/usr/local/opt/php/sbin/php-fpm --nodaemonize



SIGN!



╰─ codesign -s "Jaros CA"  --keychain ~/Library/Keychains/login.keychain-db  /usr/local/opt/php/lib/httpd/modules/libphp.so


sudo vi httpd.conf

LoadModule php_module /usr/local/opt/php/lib/httpd/modules/libphp.so "Jaros CA"


codesign -dv --verbose=4 "/usr/local/opt/php/lib/httpd/modules/libphp.so"






