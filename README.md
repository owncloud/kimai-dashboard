# kimai-dashboard

[![Build Status](https://drone.owncloud.com/api/badges/owncloud/kimai-dashboard/status.svg)](https://drone.owncloud.com/owncloud/kimai-dashboard/)
[![Docker Hub](https://img.shields.io/badge/docker-latest-blue.svg?logo=docker&logoColor=white)](https://hub.docker.com/r/owncloudops/kimai-dashboard)

This is a custom extension for Kimai written in Node.js which provides a reporting dashboard and a custom time booking interface.

## Environment variables

```Shell
KIMAI_API_URL=
KIMAI_API_USER=
KIMAI_API_TOKEN=
# relative from server.js or absolute file path
JSONDB_FILE_PATH=database/db.json
SMTP_HOST=
SMTP_PORT=465
SMTP_SECURE=TRUE
SMTP_FROM_MAIL=
SMTP_USER=
SMTP_PASS=
# the URL where the dashboard is deployed (e.g. https://dashboard.example.com)
NEXT_PUBLIC_SITE="http://localhost:3000"
# The URL of the LDAP(s) server
LDAP_URL="ldap://localhost:389/"
BASE_DN="ou=people,dc=planetexpress,dc=com"
# Admin username used for searching groups and users
LDAP_ADMIN_USERNAME="cn=admin,dc=planetexpress,dc=com"
LDAP_ADMIN_PASSWORD="GoodNewsEveryone"
LDAP_MAPPING_UID="uid"
LDAP_MAPPING_NAME="displayName"
LDAP_MAPPING_MAIL="mail"
# Used to sign the JWT token
JWT_SECRET="ewrgw3456746hwrth35678356jh"
# the LDAP groups/ou that are allowed to access the dashboard
AUTH_GROUPS_DASHBOARD="Delivering Crew,Staff"
# the LDAP groups/ou that are allowed to access the time booking page
AUTH_GROUPS_BOOKING="Office Management,Staff"
```

## Build the docker container

```Shell
docker build -t kimai-dashboard:0.2.7 --build-arg NEXT_PUBLIC_SITE=http://localhost:3000 .
```

## Deploy to kubernetes with helm

```Shell
helm upgrade -n owncloud -f deployment/values.yaml kimai-dashboard deployment/
```

## Or start container locally

```Shell
docker run --rm \
    -e KIMAI_API_URL="https://demo-stable.kimai.org" \
    -e KIMAI_API_USER="susan_super" \
    -e KIMAI_API_TOKEN="api_kitten" \
    -e SMTP_HOST="mail.example.com" \
    -e SMTP_USER="smtp_user" \
    -e SMTP_PASS="smtp_pass" \
    -e SMTP_FROM_MAIL="kimai@example.com" \
    -e SITE="http://localhost:3000" \
    -p 3000:3000 \
kimai-dashboard
```

## Test SMTP settings

If you want to quickly send a test email to verify your SMTP use:

```Shell
docker run --rm --entrypoint node \
    -e SEND_TO="test@example.com" \
    -e SMTP_HOST="mail.example.com" \
    -e SMTP_PORT="465" \
    -e SMTP_SECURE="TRUE" \
    -e SMTP_FROM_MAIL="kimai@example.com" \
    -e SMTP_USER="smtp_user" \
    -e SMTP_PASS="smtp_pass" \
kimai-dashboard backend_modules/test_smtp.js
```
