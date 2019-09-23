# For local development
Virtualhost is:
1. Security feature
2. Provides auto redirect to sub folder
3. Allows to create fake website URLs for localhosting

# How to
1. Go to `Apache>conf>extra>vhosts.conf`

Add a new host entry. Eg from WAMP. Plud WAMP also has auto creation script for this.

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

2. Go to `C:\Windows\System32\drivers\etc\hosts` and add:
```
#
127.0.0.1 localhost
::1 localhost

127.0.0.1	first.test
::1	first.test
```

3. Restart Apache or `Click on WAMP>Tools>Restart DNS`
