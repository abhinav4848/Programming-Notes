# Force redirect to https
```apache
RewriteEngine On 
RewriteCond %{SERVER_PORT} 80 
RewriteRule ^(.*)$ https://www.yourdomain.com/$1 [R,L]
```

# Use a subdirectory to load files from
Eg: if people go to `abhinavkr.com/hahaha`, the url changes to `https://abhinavkr.com/blog/hahaha`.

```apache
RewriteEngine on

# Change yourdomain.com to be your primary domain.
RewriteCond %{HTTP_HOST} ^(www.)?abhinavkr.com$

# Change 'subfolder' to be the folder you will use for your primary domain.
RewriteCond %{REQUEST_URI} !^/blog/

# Don't change this line.
RewriteCond %{REQUEST_FILENAME} !-f 
RewriteCond %{REQUEST_FILENAME} !-d

# Change 'subfolder' to be the folder you will use for your primary domain.
RewriteRule ^(.*)$ /blog/$1

# Change yourdomain.com to be your primary domain again. 
# Change 'subfolder' to be the folder you will use for your primary domain 
# followed by / then the main file for your site, index.php, index.html, etc.
RewriteCond %{HTTP_HOST} ^(www.)?abhinavkr.com$ 
RewriteRule ^(/)?$ index.php [L]
# /index.php shows url as abhinavkr.com
# index.php shows url as abhinavkr.com/blog/
```