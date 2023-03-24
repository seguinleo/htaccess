# htaccess
Up to date and safe .htaccess template to improve security and avoid issues for Apache2.

``` apacheconf
# Force UTF-8 encoding
AddDefaultCharset utf-8

# Force JavaScript type to avoid old application/x-javascript
AddType text/javascript .js

# Redirect error pages
ErrorDocument 401 https://YOUR_DOMAIN/error/401/
ErrorDocument 403 https://YOUR_DOMAIN/error/403/
ErrorDocument 404 https://YOUR_DOMAIN/error/404/
ErrorDocument 500 https://YOUR_DOMAIN/error/500/

# Block access to certain files
<FilesMatch ".(htaccess|htpasswd|ini|phps|fla|psd|log|sh)$">
Order Allow,Deny
Deny from all
</FilesMatch>

# Disable server signature
ServerSignature Off

# Disable directory browsing
Options -Indexes

# Enable protection against query injection attacks
<IfModule mod_security.c>
SecFilterEngine On
SecFilterScanPOST On
SecFilterCheckURLEncoding On
SecFilterCheckCookieFormat On
SecFilterCheckUnicodeEncoding On
SecFilterNormalizeCookies On
SecFilterNormalizeEncoding On
</IfModule>

# Limit HTTP connections attempts (ex: .htpasswd)
<IfModule mod_auth.c>
<Limit LOGIN>
Order Allow,Deny
Allow from all
Deny from env=too_many_attempts
</Limit>
</IfModule>

# Custom headers (cache, security, etc.)
<IfModule mod_headers.c>
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
Header unset expires
Header unset platform
Header unset pragma
Header unset server
Header unset x-powered-by

# Remove CSP for non-HTML resources
<FilesMatch "\.(appcache|atom|bbaw|bmp|crx|css|cur|eot|f4[abpv]|flv|geojson|gif|htc|ic[os]|jpe?g|json(ld)?|m4[av]|manifest|map|markdown|md|mp4|oex|og[agv]|opus|otf|png|rdf|rss|safariextz|swf|topojson|tt[cf]|txt|vcard|vcf|vtt|webapp|web[mp]|webmanifest|woff2?|xloc|xpi)$">
Header unset Content-Security-Policy
</FilesMatch>
</IfModule>

# Enable RewriteEngine
RewriteEngine On

# Disable TRACE and TRACK requests
RewriteCond %{REQUEST_METHOD} ^(TRACE|TRACK)
RewriteRule .* - [F]

# Redirect www to non-www
RewriteCond %{HTTP_HOST} ^www\.(.*)$ [NC]
RewriteRule ^(.*)$ https://%1/$1 [R=301,L]

# Rewrite index.html to /
RewriteCond %{THE_REQUEST} ^.*/index\.html
RewriteRule ^(.*)index.html$ https://%{HTTP_HOST}/$1 [R=301,L]

# Rewrite index.php to /
RewriteCond %{THE_REQUEST} ^.*/index\.php
RewriteRule ^(.*)index.php$ https://%{HTTP_HOST}/$1 [R=301,L]
```

## DO NOT USE ANYMORE:

``` apacheconf
X-XSS-Protection
X-Frame-Options
Pragma
```

[Source](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)

PouletEnSlip ©
