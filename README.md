# htaccess
Up to date and safe .htaccess template to improve security and avoid issues for Apache2.

``` apacheconf
# Disable server signature
ServerSignature Off

# Disable directory browsing
Options -Indexes

# Redirect error pages
ErrorDocument 401 /error/401/
ErrorDocument 403 /error/403/
ErrorDocument 404 /error/404/

# Block access to certain files
<FilesMatch "(^\.ht|\.htaccess|\.htpasswd|\.ini|\.phps|\.log|\.sh|\.env|\.bak|\.config|\.sql|\.xml|config.php)">
Require all denied
</FilesMatch>

# Add custom headers (cache, security, permissions, etc.)
<IfModule mod_headers.c>
Header set Access-Control-Allow-Origin: https://YOUR_DOMAIN/
# ⚠️ modify the one below according to your needs
Header set Cache-Control: no-cache
# ⚠️ modify the one below according to your needs
Header set Content-Security-Policy: upgrade-insecure-requests; default-src 'none'; base-uri 'none'; child-src 'none'; connect-src 'self'; frame-src 'none'; frame-ancestors 'none'; font-src 'self'; form-action 'none'; img-src 'self'; manifest-src 'none'; media-src 'none'; object-src 'none'; script-src 'self'; style-src 'self'; worker-src 'none'
Header set Cross-Origin-Embedder-Policy: require-corp
Header set Cross-Origin-Resource-Policy: cross-origin
Header set Cross-Origin-Opener-Policy: same-origin
# ⚠️ modify the one below according to your needs
Header set Permissions-Policy: camera=(), display-capture=(), fullscreen=(), microphone=()
Header set Referrer-Policy: strict-origin-when-cross-origin
Header set Strict-Transport-Security: max-age=63072000; includeSubDomains; preload
Header set X-Content-Type-Options: nosniff
Header set X-Frame-Options: DENY # <- not required if CSP frame-ancestors 'none';
# Remove useless headers
Header unset platform
Header unset pragma
Header unset server
Header unset x-powered-by
Header unset x-turbo-charged-by

# Enable RewriteEngine
RewriteEngine On

# Disable TRACE and TRACK requests (Cloudflare disables it by default)
RewriteCond %{REQUEST_METHOD} ^(TRACE|TRACK)
RewriteRule .* - [F,L]

# Redirect www to non-www
RewriteCond %{HTTP_HOST} ^www\.(.*)$ [NC]
RewriteRule ^(.*)$ https://%1/$1 [R=301,L]

# Rewrite /index.html to /
RewriteCond %{THE_REQUEST} ^.*/index\.html
RewriteRule ^(.*)index.html$ https://%{HTTP_HOST}/$1 [R=301,L]

# Rewrite /index.php to /
RewriteCond %{THE_REQUEST} ^.*/index\.php
RewriteRule ^(.*)index.php$ https://%{HTTP_HOST}/$1 [R=301,L]
```

## DO NOT USE ANYMORE:

``` apacheconf
X-XSS-Protection
Pragma
Feature-Policy
```

[Source](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)
