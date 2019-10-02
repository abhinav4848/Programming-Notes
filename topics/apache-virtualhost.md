# For local development
Virtualhost is:
1. Security feature
2. Provides auto redirect to sub folder
3. Allows to create fake website URLs for localhosting

# How To
1. Add a new host entry:

	**Wamp**:

	Go to `Apache>conf>extra>vhosts.conf` (Plus WAMP also has auto creation script for this) and add an entry like:

	```xml
	<VirtualHost *:80>
		ServerName first.test
		DocumentRoot "c:/wamp64/www/firstapp/public"
		<Directory  "c:/wamp64/www/firstapp/public/">
			Options +Indexes +Includes +FollowSymLinks +MultiViews
			AllowOverride All
			Require local
		</Directory>
	</VirtualHost>
	```

	**XAMPP**:

	Go to `C:\xampp\apache\conf\extra\httpd-vhosts.conf` and add an entry like
	```xml
	<VirtualHost *:80>
		DocumentRoot "C:/xampp/htdocs"
		ServerName localhost
	</VirtualHost>

	<VirtualHost *:80>
		DocumentRoot "C:/xampp/htdocs/firstapp.test"
		ServerName firstapp.test
	</VirtualHost>
	```

2. Go to `C:\Windows\System32\drivers\etc` on windows or `etc` folder on Linux, open file `hosts` and add:
	```
	#
	127.0.0.1 localhost
	::1 localhost

	127.0.0.1	first.test
	::1	first.test
	```

3. Restart Apache or `Click on WAMP>Tools>Restart DNS`
