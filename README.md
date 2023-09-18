Up to date and safe .htaccess/apache2.conf template to improve security and avoid issues.

``` apacheconf
# Limit server tokens
ServerTokens Prod

# Disable server signature
ServerSignature Off

# Disable directory browsing
Options -Indexes

# Disable TRACE and TRACK requests
TraceEnable Off

# Redirect error pages
ErrorDocument 401 /error/401/
ErrorDocument 403 /error/403/
ErrorDocument 404 /error/404/

# Add custom headers (cache, security, permissions, etc.)
Header set Access-Control-Allow-Origin: https://YOUR_DOMAIN/
# ⚠️ modify the one below according to your needs
Header set Cache-Control: no-cache, must-revalidate
# ⚠️ modify the one below according to your needs
Header set Content-Security-Policy: upgrade-insecure-requests; default-src 'none'; base-uri 'none'; child-src 'none'; connect-src 'self'; frame-ancestors 'none'; frame-src 'none'; font-src 'self'; form-action 'self'; img-src 'self'; manifest-src 'self'; media-src 'none'; object-src 'none'; script-src 'self'; script-src-attr 'none'; script-src-elem 'self'; style-src 'self'; worker-src 'self'
Header set Cross-Origin-Embedder-Policy: require-corp
Header set Cross-Origin-Resource-Policy: cross-origin
Header set Cross-Origin-Opener-Policy: same-origin
# ⚠️ modify the one below according to your needs
Header set Permissions-Policy: camera=(), display-capture=(), fullscreen=(), geolocation=(), interest-cohort=(), microphone=(), payment=(), usb=()
Header set Referrer-Policy: strict-origin-when-cross-origin
Header set Strict-Transport-Security: max-age=63072000; includeSubDomains; preload # <- submit your website https://hstspreload.org/
Header set X-Content-Type-Options: nosniff
Header set X-Frame-Options: DENY # <- not required if CSP frame-ancestors 'none';

# Remove useless/obsolete headers
Header unset platform
Header unset pragma
Header unset server
Header unset x-powered-by
Header unset x-turbo-charged-by

# Enable RewriteEngine
RewriteEngine On

# Redirect www to non-www
RewriteCond %{HTTP_HOST} ^www\.(.*)$ [NC]
RewriteRule ^(.*)$ https://%1/$1 [R=301,L]

# Rewrite /index.html to /
RewriteCond %{THE_REQUEST} ^.*/index\.html
RewriteRule ^(.*)index.html$ https://%{HTTP_HOST}/$1 [R=301,L]

# Rewrite /index.php to /
RewriteCond %{THE_REQUEST} ^.*/index\.php
RewriteRule ^(.*)index.php$ https://%{HTTP_HOST}/$1 [R=301,L]

# Block access to certain files
<FilesMatch "(^\.ht|\.htaccess|\.htpasswd|\.ini|\.phps|\.log|\.sh|\.env|\.bak|\.config|\.sql|config.php)">
  Require all denied
</FilesMatch>

# Enable Brotli
<IfModule mod_brotli.c>
	AddOutputFilterByType BROTLI_COMPRESS text/html text/css text/javascript
</IfModule>
```

### DO NOT USE ANYMORE:

``` apacheconf
X-XSS-Protection
Pragma
Feature-Policy
```

[Source](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)
