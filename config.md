# Odoo Configuration Parameters - Detailed Guide

## Common Options

### config (`-c`)
- **Purpose:** Specify an alternate configuration file path
- **Default:** System default (`~/.odoorc` on Linux, `odoo.conf` on Windows)
- **Usage:** `--config /path/to/custom.conf`
- **Details:** Allows using a custom configuration file instead of the default location

### save (`-s`)
- **Purpose:** Save current configuration to the default config file
- **Default:** False
- **Usage:** `--save`
- **Details:** Writes all current settings to `~/.odoorc` or `~/.openerp_serverrc`, useful for persisting command-line options

### init (`-i`)
- **Purpose:** Install one or more modules during startup
- **Default:** None
- **Usage:** `--init base,sale,purchase` or `--init all`
- **Details:** Comma-separated list of modules to install. Requires `-d` (database name). Use "all" to install all available modules

### update (`-u`)
- **Purpose:** Update one or more modules during startup
- **Default:** None
- **Usage:** `--update sale,purchase` or `--update all`
- **Details:** Comma-separated list of modules to update. Requires `-d`. Updates module data, views, and code

### without_demo
- **Purpose:** Disable loading demo data for modules being installed
- **Default:** False
- **Usage:** `--without-demo`
- **Details:** Prevents installation of sample/demo data. Useful for production environments

### import_partial (`-P`)
- **Purpose:** Enable crash recovery for large data imports
- **Default:** Empty string
- **Usage:** `--import-partial /path/to/state_file`
- **Details:** Stores intermediate import states, allowing recovery from crashes during big data imports

### pidfile
- **Purpose:** File where the server process ID will be stored
- **Default:** None
- **Usage:** `--pidfile /var/run/odoo.pid`
- **Details:** Useful for init scripts and process management tools

### addons_path
- **Purpose:** Additional directories containing Odoo addons/modules
- **Default:** Auto-detected (`addons`, `../addons`)
- **Usage:** `--addons-path /custom/addons,/other/addons`
- **Details:** Comma-separated paths where Odoo looks for modules. Searched in order

### upgrade_path
- **Purpose:** Additional directories containing upgrade scripts
- **Default:** Empty string
- **Usage:** `--upgrade-path /path/to/upgrade/scripts`
- **Details:** Points to directories with migration scripts for module upgrades

### pre_upgrade_scripts
- **Purpose:** Run specific upgrade scripts before module loading with `-u`
- **Default:** Empty string
- **Usage:** `--pre-upgrade-scripts /path/to/script1.py,/path/to/script2.py`
- **Details:** Executes custom scripts before the standard update process

### server_wide_modules (`--load`)
- **Purpose:** Comma-separated list of server-wide modules to load
- **Default:** `base,web`
- **Usage:** `--load base,web,my_custom_module`
- **Details:** Modules loaded at server startup, available across all databases

### data_dir (`-D`)
- **Purpose:** Directory where Odoo stores its data files
- **Default:** Platform-specific (e.g., `~/.local/share/Odoo` on Linux)
- **Usage:** `--data-dir /opt/odoo/data`
- **Details:** Contains filestore, sessions, and other persistent data

---

## HTTP Service Configuration

### http_interface
- **Purpose:** Network interface for HTTP services to bind to
- **Default:** Empty (all interfaces - `0.0.0.0`)
- **Usage:** `--http-interface 127.0.0.1`
- **Details:** Restricts HTTP access to specific network interfaces. Empty = all interfaces

### http_port (`-p`)
- **Purpose:** Port number for the main HTTP service
- **Default:** 8069
- **Usage:** `--http-port 8080`
- **Details:** Main port where Odoo web interface and API are accessible

### gevent_port
- **Purpose:** Port for the gevent worker process
- **Default:** 8072
- **Usage:** `--gevent-port 8073`
- **Details:** Used for long-polling and real-time features when using gevent

### http_enable (`--no-http`)
- **Purpose:** Enable/disable HTTP and Longpolling services
- **Default:** True
- **Usage:** `--no-http`
- **Details:** When disabled, Odoo runs without web interface (CLI/shell mode only)

### proxy_mode
- **Purpose:** Enable reverse proxy WSGI wrappers
- **Default:** False
- **Usage:** `--proxy-mode`
- **Details:** Enables header rewriting for X-Forwarded-* headers. Only use behind trusted proxies

### x_sendfile
- **Purpose:** Enable X-Sendfile/X-Accel-Redirect for file delivery
- **Default:** False
- **Usage:** `--x-sendfile`
- **Details:** Delegates file serving to web server (Apache/Nginx) for better performance

---

## Web Interface Configuration

### dbfilter
- **Purpose:** Regular expression for filtering available databases
- **Default:** Empty string (all databases visible)
- **Usage:** `--db-filter ^production.*`
- **Details:** Controls which databases appear in login screen. Supports `%d` (domain) and `%h` (host) placeholders

---

## Testing Configuration

### test_file
- **Purpose:** Launch a specific Python test file
- **Default:** False
- **Usage:** `--test-file /path/to/test.py`
- **Details:** Runs a single test file instead of standard module tests

### test_enable
- **Purpose:** Enable unit tests during module installation/update
- **Default:** False
- **Usage:** `--test-enable`
- **Details:** Automatically enabled when test_tags is set

### test_tags
- **Purpose:** Filter tests by tags and specifications
- **Default:** None
- **Usage:** `--test-tags :TestClass.test_method,/module_name,standard`
- **Details:** Complex filtering syntax:
    - Prefix excludes tests
    - Format: `[-][tag][/module][:class][.method][[params]]`
    - Examples: `standard`, `-external`, `/account:TestAccount.test_balance`

### screencasts
- **Purpose:** Directory for storing test screencasts
- **Default:** None
- **Usage:** `--screencasts /var/log/odoo/screencasts`
- **Details:** Videos of browser tests go in `DIR/{db_name}/screencasts/`

### screenshots
- **Purpose:** Directory for storing test screenshots
- **Default:** `/tmp/odoo_tests`
- **Usage:** `--screenshots /var/log/odoo/screenshots`
- **Details:** Test failure screenshots go in `DIR/{db_name}/screenshots/`

---

## Logging Configuration

### logfile
- **Purpose:** File where server logs will be stored
- **Default:** None (stdout)
- **Usage:** `--logfile /var/log/odoo.log`
- **Details:** When not set, logs go to stdout/stderr

### syslog
- **Purpose:** Send logs to syslog server
- **Default:** False
- **Usage:** `--syslog`
- **Details:** Cannot be used with logfile option simultaneously

### log_handler
- **Purpose:** Setup specific log handlers at different levels
- **Default:** `[':INFO']`
- **Usage:** `--log-handler odoo.orm:DEBUG --log-handler werkzeug:WARNING`
- **Details:** Format: `PREFIX:LEVEL`. Empty prefix = root logger. Can be repeated

### log_web
- **Purpose:** Shortcut for HTTP request/response logging
- **Default:** Not set
- **Usage:** `--log-web`
- **Details:** Equivalent to `--log-handler=odoo.http:DEBUG`

### log_sql
- **Purpose:** Shortcut for SQL query logging
- **Default:** Not set
- **Usage:** `--log-sql`
- **Details:** Equivalent to `--log-handler=odoo.sql_db:DEBUG`

### log_db
- **Purpose:** Database for storing logs
- **Default:** False
- **Usage:** `--log-db log_database`
- **Details:** Stores logs in specified database instead of files

### log_db_level
- **Purpose:** Minimum log level for database logging
- **Default:** `'warning'`
- **Usage:** `--log-db-level error`
- **Details:** Only logs at this level or higher go to the log database

### log_level
- **Purpose:** Overall logging level
- **Default:** `'info'`
- **Usage:** `--log-level debug`
- **Details:** Available levels: info, debug, warn, error, critical, notset

---

## SMTP Configuration

### email_from
- **Purpose:** Default email address for outgoing emails
- **Default:** False
- **Usage:** `--email-from noreply@company.com`
- **Details:** Used as sender address when no specific sender is configured

### from_filter
- **Purpose:** Email address filter for SMTP usage
- **Default:** False
- **Usage:** `--from-filter @company.com`
- **Details:** Restricts which email addresses can use the SMTP configuration

### smtp_server
- **Purpose:** SMTP server hostname or IP
- **Default:** `'localhost'`
- **Usage:** `--smtp smtp.gmail.com`
- **Details:** Server used for sending emails

### smtp_port
- **Purpose:** SMTP server port
- **Default:** 25
- **Usage:** `--smtp-port 587`
- **Details:** Common ports: 25 (plain), 587 (STARTTLS), 465 (SSL)

### smtp_ssl
- **Purpose:** Enable SSL/STARTTLS for SMTP connections
- **Default:** False
- **Usage:** `--smtp-ssl`
- **Details:** Encrypts email transmission

### smtp_user
- **Purpose:** SMTP authentication username
- **Default:** False
- **Usage:** `--smtp-user myemail@gmail.com`
- **Details:** Username for SMTP authentication

### smtp_password
- **Purpose:** SMTP authentication password
- **Default:** False
- **Usage:** `--smtp-password mypassword`
- **Details:** Password for SMTP authentication

### smtp_ssl_certificate_filename
- **Purpose:** SSL certificate file for SMTP authentication
- **Default:** False
- **Usage:** `--smtp-ssl-certificate-filename /path/to/cert.pem`
- **Details:** Client certificate for SSL authentication

### smtp_ssl_private_key_filename
- **Purpose:** SSL private key file for SMTP authentication
- **Default:** False
- **Usage:** `--smtp-ssl-private-key-filename /path/to/key.pem`
- **Details:** Private key corresponding to the SSL certificate

---

## Database Options

### db_name (`-d`)
- **Purpose:** Default database name
- **Default:** False
- **Usage:** `--database production_db`
- **Details:** Required for many operations like module installation/updates

### db_user (`-r`)
- **Purpose:** PostgreSQL username
- **Default:** False
- **Usage:** `--db_user odoo`
- **Details:** Database user for PostgreSQL connections

### db_password (`-w`)
- **Purpose:** PostgreSQL password
- **Default:** False
- **Usage:** `--db_password secretpassword`
- **Details:** Password for database authentication

### pg_path
- **Purpose:** Path to PostgreSQL executables
- **Default:** None
- **Usage:** `--pg_path /usr/bin`
- **Details:** Used for database operations like createdb, dropdb

### db_host
- **Purpose:** PostgreSQL server hostname/IP
- **Default:** False (Unix socket)
- **Usage:** `--db_host localhost`
- **Details:** Database server location

### db_replica_host
- **Purpose:** Read replica PostgreSQL server hostname/IP
- **Default:** False
- **Usage:** `--db_replica_host replica.example.com`
- **Details:** Used for read-only operations to reduce load on main DB

### db_port
- **Purpose:** PostgreSQL server port
- **Default:** False (PostgreSQL default: 5432)
- **Usage:** `--db_port 5432`
- **Details:** Port for database connections

### db_replica_port
- **Purpose:** Read replica PostgreSQL server port
- **Default:** False
- **Usage:** `--db_replica_port 5433`
- **Details:** Port for replica database connections

### db_sslmode
- **Purpose:** SSL connection mode for PostgreSQL
- **Default:** `'prefer'`
- **Usage:** `--db_sslmode require`
- **Details:** Options: disable, allow, prefer, require, verify-ca, verify-full

### db_maxconn
- **Purpose:** Maximum PostgreSQL connections
- **Default:** 64
- **Usage:** `--db_maxconn 100`
- **Details:** Connection pool size for main database operations

### db_maxconn_gevent
- **Purpose:** Maximum PostgreSQL connections for gevent worker
- **Default:** False (uses db_maxconn)
- **Usage:** `--db_maxconn_gevent 32`
- **Details:** Separate connection limit for gevent worker

### db_template
- **Purpose:** PostgreSQL template database for new databases
- **Default:** `'template0'`
- **Usage:** `--db-template template1`
- **Details:** Template used when creating new databases

---

## Internationalization Options

### load_language
- **Purpose:** Languages for translations to load
- **Default:** None
- **Usage:** `--load-language en_US,fr_FR,es_ES`
- **Details:** Loads translation files for specified languages

### language (`-l`)
- **Purpose:** Language for translation operations
- **Default:** None
- **Usage:** `--language fr_FR`
- **Details:** Required for import/export operations

### translate_out (`--i18n-export`)
- **Purpose:** Export translations to file
- **Default:** None
- **Usage:** `--i18n-export /path/to/translations.po`
- **Details:** Exports to CSV, PO, or TGZ format. Requires `-d`

### translate_in (`--i18n-import`)
- **Purpose:** Import translations from file
- **Default:** None
- **Usage:** `--i18n-import /path/to/translations.po`
- **Details:** Imports from CSV or PO format. Requires `-l` and `-d`

### overwrite_existing_translations
- **Purpose:** Overwrite existing translation terms
- **Default:** False
- **Usage:** `--i18n-overwrite`
- **Details:** Updates existing translations during import or module update

### translate_modules
- **Purpose:** Modules to export for translation
- **Default:** `['all']`
- **Usage:** `--modules sale,purchase`
- **Details:** Comma-separated list of modules for translation export

---

## Security Options

### list_db (`--no-database-list`)
- **Purpose:** Enable/disable database list access
- **Default:** True
- **Usage:** `--no-database-list`
- **Details:** Hides database list and manager. Improves security by hiding database names

---

## Advanced Options

### dev_mode
- **Purpose:** Enable developer mode features
- **Default:** None
- **Usage:** `--dev all` or `--dev reload,qweb`
- **Details:** Options: all, reload (auto-reload), qweb (QWeb debugging), xml (XML validation)

### shell_interface
- **Purpose:** Preferred REPL for shell mode
- **Default:** None
- **Usage:** `--shell-interface ipython`
- **Details:** Options: ipython, ptpython, bpython, python

### stop_after_init
- **Purpose:** Stop server after initialization
- **Default:** False
- **Usage:** `--stop-after-init`
- **Details:** Useful for batch operations like module installation

### osv_memory_count_limit
- **Purpose:** Limit on virtual osv_memory table records
- **Default:** 0 (no limit)
- **Usage:** `--osv-memory-count-limit 1000`
- **Details:** Prevents memory leaks from wizard/temporary records

### transient_age_limit
- **Purpose:** Time limit for TransientModel records (hours)
- **Default:** 1.0
- **Usage:** `--transient-age-limit 2.5`
- **Details:** Auto-cleanup of wizard records older than this limit

### max_cron_threads
- **Purpose:** Maximum concurrent cron job threads
- **Default:** 2
- **Usage:** `--max-cron-threads 4`
- **Details:** Controls parallel execution of scheduled tasks

### limit_time_worker_cron
- **Purpose:** Maximum cron worker lifetime (seconds)
- **Default:** 0 (disabled)
- **Usage:** `--limit-time-worker-cron 3600`
- **Details:** Restarts cron workers periodically to prevent memory leaks

### unaccent
- **Purpose:** Enable unaccent extension for new databases
- **Default:** False
- **Usage:** `--unaccent`
- **Details:** Improves search by ignoring accents/diacritics

### geoip_city_db
- **Purpose:** Path to GeoIP City database file
- **Default:** `/usr/share/GeoIP/GeoLite2-City.mmdb`
- **Usage:** `--geoip-city-db /custom/path/GeoLite2-City.mmdb`
- **Details:** Used for geographic IP address resolution

### geoip_country_db
- **Purpose:** Path to GeoIP Country database file
- **Default:** `/usr/share/GeoIP/GeoLite2-Country.mmdb`
- **Usage:** `--geoip-country-db /custom/path/GeoLite2-Country.mmdb`
- **Details:** Used for country-level IP address resolution

---

## Multiprocessing Options (POSIX only)

### workers
- **Purpose:** Number of worker processes
- **Default:** 0 (single process)
- **Usage:** `--workers 4`
- **Details:** 0 disables prefork mode. Use for production with multiple CPU cores

### limit_memory_soft
- **Purpose:** Soft memory limit per worker (bytes)
- **Default:** 2048 * 1024 * 1024 (2GB)
- **Usage:** `--limit-memory-soft 1073741824`
- **Details:** Worker restarted after current request when limit reached

### limit_memory_soft_gevent
- **Purpose:** Soft memory limit for gevent worker (bytes)
- **Default:** False (uses limit_memory_soft)
- **Usage:** `--limit-memory-soft-gevent 536870912`
- **Details:** Separate limit for gevent worker process

### limit_memory_hard
- **Purpose:** Hard memory limit per worker (bytes)
- **Default:** 2560 * 1024 * 1024 (2.5GB)
- **Usage:** `--limit-memory-hard 2147483648`
- **Details:** Memory allocation fails when limit reached

### limit_memory_hard_gevent
- **Purpose:** Hard memory limit for gevent worker (bytes)
- **Default:** False (uses limit_memory_hard)
- **Usage:** `--limit-memory-hard-gevent 1073741824`
- **Details:** Hard limit for gevent worker memory

### limit_time_cpu
- **Purpose:** CPU time limit per request (seconds)
- **Default:** 60
- **Usage:** `--limit-time-cpu 120`
- **Details:** Kills request if CPU usage exceeds limit

### limit_time_real
- **Purpose:** Real time limit per request (seconds)
- **Default:** 120
- **Usage:** `--limit-time-real 300`
- **Details:** Kills request if wall-clock time exceeds limit

### limit_time_real_cron
- **Purpose:** Real time limit for cron jobs (seconds)
- **Default:** -1 (uses limit_time_real)
- **Usage:** `--limit-time-real-cron 600`
- **Details:** Separate time limit for scheduled tasks. Set to 0 for no limit

### limit_request
- **Purpose:** Maximum requests per worker
- **Default:** 65536
- **Usage:** `--limit-request 10000`
- **Details:** Worker restarted after handling this many requests

---

## Internal Options (Not exposed on command line)

### admin_passwd
- **Purpose:** Master/admin password for database operations
- **Default:** `'admin'`
- **Details:** Used for database management operations, creating/dropping databases

### csv_internal_sep
- **Purpose:** CSV field separator
- **Default:** `','`
- **Details:** Character used to separate fields in CSV exports/imports

### publisher_warranty_url
- **Purpose:** URL for publisher warranty service
- **Default:** `'http://services.odoo.com/publisher-warranty/'`
- **Details:** Used for Odoo Enterprise features and support

### reportgz
- **Purpose:** Enable report compression
- **Default:** False
- **Details:** Compresses generated reports to save bandwidth

### root_path
- **Purpose:** Odoo installation root path
- **Default:** Auto-detected
- **Details:** Base directory of Odoo installation

### websocket_keep_alive_timeout
- **Purpose:** WebSocket keep-alive timeout (seconds)
- **Default:** 3600
- **Details:** How long to keep WebSocket connections alive

### websocket_rate_limit_burst
- **Purpose:** WebSocket rate limit burst size
- **Default:** 10
- **Details:** Number of rapid requests allowed before rate limiting

### websocket_rate_limit_delay
- **Purpose:** WebSocket rate limit delay (seconds)
- **Default:** 0.2
- **Details:** Delay imposed between requests when rate limiting

---

## Configuration Priority

Configuration values are applied in this order (highest to lowest priority):

1. Command-line arguments
2. Environment variables (`ODOO_RC`, `OPENERP_SERVER`)
3. Configuration file (specified with `-c` or default location)
4. Built-in defaults

> Each parameter can be set through any of these methods, with command-line arguments taking precedence over all others.