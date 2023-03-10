# htaccess
Up to date .htaccess template to improve security and avoid issues for Apache.

``` apacheconf
# Force UTF-8 encoding
AddDefaultCharset utf-8

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
Header set Cache-Control: no-cache
Header set Content-Security-Policy: upgrade-insecure-requests; default-src 'none'; base-uri 'none'; child-src 'none'; connect-src 'self'; frame-src 'none'; frame-ancestors 'none'; font-src 'self'; img-src 'self'; media-src 'none'; object-src 'none'; script-src 'self'; style-src 'self'; worker-src 'none'
Header set Cross-Origin-Embedder-Policy: require-corp
Header set Cross-Origin-Resource-Policy: cross-origin
Header set Cross-Origin-Opener-Policy: same-origin
Header set Permissions-Policy: accelerometer=(), camera=(), gyroscope=(), microphone=(), payment=()
Header set Referrer-Policy: strict-origin-when-cross-origin
Header set Strict-Transport-Security: max-age=63072000; includeSubDomains; preload
Header set X-Content-Type-Options: nosniff
Header unset platform
Header unset pragma
Header unset server
Header unset x-powered-by
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

## DO NOT USE ANYMORE

``` apacheconf
X-XSS-Protection
X-Frame-Options
Pragma
```

[See also](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)

PouletEnSlip ©
