---
title: Apache tips
date: 2022-03-30 09:00:00 HH:MM:SS +0100
categories: [System, Linux, Apache]
tags: [system, linux, shell, tips, apache]
---

### Aliases

Here consider `http://myServer` as `ServerRoot`.

#### Simple aliases

```properties
Alias /alias sub/path
```

Here `http://myServer/alias` and `http://myServer/sub/path` are now the same.

#### Script aliases

```properties
ScriptAlias /cgi-bin https/cgi-bin
```

Here `http://myServer/cgi-bin` and `http://myServer/https/cgi-bin` are now the same.

Remark: everything under the script alias will be executed, so we can't put CSS or JS files inside.

#### Location aliases

```properties
<Location "/ping">
    Alias "http/ping.txt"
</Location>
```

Here `http://myServer/ping` and `http://myServer/http/ping.txt` are now the same. Useful for a healthcheck for example.

### Apache server that listens to several ports

Simply one ServerRoot, several Listen port, ie.:

```properties
# We define the server root path
ServerRoot "/myPath"

# We define the ports the Apache instance listens to
Listen 8080
Listen 8443
```

Then several virtual hosts:

```properties
<VirtualHost *:8080>
    DocumentRoot http
    ErrorLog logs/error_log_http
    LogFormat "%h %l %u %t \"%r\" %>s %b" common
    CustomLog logs/access_log_http common
    ServerName myServer
    Options -Indexes
</VirtualHost>

<VirtualHost *:8443>
    DocumentRoot https
    ErrorLog logs/error_log_https
    LogFormat "%h %l %u %t \"%r\" %>s %b" common
    CustomLog logs/access_log_https common
    ServerName myServer
    Options -Indexes
</VirtualHost>
```

Remark: aliases, script aliases, etc. must be set at the virtual host level, or they will be global (that means ok for every virtual host).

More complete example:

```properties
# Unique server root
ServerRoot "/myBasePath/apache//myCustomPath"

# We define the ports the Apache instance listens to
Listen 8080
Listen 8443

# Modules
LoadModule unixd_module /etc/httpd/modules/mod_unixd.so
LoadModule mpm_event_module /etc/httpd/modules/mod_mpm_event.so
LoadModule autoindex_module /etc/httpd/modules/mod_autoindex.so
LoadModule dir_module /etc/httpd/modules/mod_dir.so
LoadModule alias_module /etc/httpd/modules/mod_alias.so
LoadModule cgi_module /etc/httpd/modules/mod_cgi.so
LoadModule auth_basic_module /etc/httpd/modules/mod_auth_basic.so
LoadModule authn_file_module /etc/httpd/modules/mod_authn_file.so
LoadModule authn_core_module /etc/httpd/modules/mod_authn_core.so
LoadModule authz_core_module /etc/httpd/modules/mod_authz_core.so
LoadModule authz_host_module /etc/httpd/modules/mod_authz_host.so
LoadModule authz_user_module /etc/httpd/modules/mod_authz_user.so
LoadModule ldap_module /etc/httpd/modules/mod_ldap.so
LoadModule authnz_ldap_module /etc/httpd/modules/mod_authnz_ldap.so
LoadModule mime_module /etc/httpd/modules/mod_mime.so
LoadModule headers_module /etc/httpd/modules/mod_headers.so
LoadModule log_config_module /etc/httpd/modules/mod_log_config.so
LoadModule systemd_module /etc/httpd/modules/mod_systemd.so

# User & group used to start this instance of httpd service
User myUser
Group myGroup

# Virtual host definitions
# First host accessible through HTTP
<VirtualHost *:8080>
    DocumentRoot http
    ErrorLog logs/error_log_http
    LogFormat "%h %l %u %t \"%r\" %>s %b" common
    CustomLog logs/access_log_http common
    ServerName myServer
    Options -Indexes
    # URL aliases (the document root will be accessible via /subPath/foo)
    Alias /subPath/foo http/foo
    Alias /subPath http/foo
</VirtualHost>

# Other host accessible through HTTPS
<VirtualHost *:8443>
    DocumentRoot https
    ErrorLog logs/error_log_https
    LogFormat "%h %l %u %t \"%r\" %>s %b" common
    CustomLog logs/access_log_https common
    ServerName myServer
    Options -Indexes
    # Warning: everything under ScriptAlias is interpreted as a script, so pictures etc. can't be in this kind of location
    # Script alias for CGI
    ScriptAlias /cgi-bin https/cgi-bin
    # Script alias for tools
    ScriptAlias /tools https/cgi-bin/tools
</VirtualHost>

# Disable to download sub .htaccess files
<FilesMatch "^\.ht">
    Require all denied
</FilesMatch>

# Alias to a single file: ping endpoint for healthcheck
<Location "/ping">
    Alias "http/ping.txt"
</Location>

# Tools directory with LDAP authentication and group appartenance check
<Directory /myBasePath/apache//myCustomPath/https/cgi-bin/tools>
  AuthType Basic
  AuthName "ARCA LDAP authentication"
  AuthBasicProvider ldap
  AuthLDAPURL "ldap://myLdapServer:389/ou=myOtherOu,o=myO???(&(objectClass=person)(!(accountsuspended=1))(!(passwordexpirationtime=19700101000000Z)))" NONE
  AuthLDAPBindDN "uid=myUid,ou=myOu,ou=myOtherOu,o=myO"
  AuthLDAPBindPassword "myPassword"
  Require ldap-group cn=use_apache_tools,cn=/myCustomPath,ou=Applications,ou=myRightsOu,o=myO
  AddType text/css .css
  AddType text/javascript .js
</Directory>

# CGI directory: no direct access, only in subdirectories
<Directory /myBasePath/apache//myCustomPath/https/cgi-bin>
  Require all denied
</Directory>
```
