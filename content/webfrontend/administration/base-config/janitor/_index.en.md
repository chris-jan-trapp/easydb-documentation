---
title: "208 - Janitor"
menu:
  main:
    name: "Janitor"
    identifier: "webfrontend/administration/base-config/janitor"
    parent: "webfrontend/administration/base-config"
---

# Janitor

Settings for purging deleted objects, user data and assets.

> For general technical information about the Janitor, click [here](/en/technical/janitor).

Time intervalls can be specified in minutes (with the suffix `'m'`), hours (with the suffix `'h'`) or days (with the suffix `'d'`).

For example, 5 minutes would be written as `'5m'`, 24 hours could be written either as `'24h'` or `'1d'`.

### General Janitor Settings

* **Janitor-Intervall**:
  * Specify how long the janitor should wait between two runs
  * Minimum duration: 1 minute (`'1m'`)
  * Default duration: 10 minutes (`'10m'`)

### Settings for deleting user sessions from the database

* **Expiration time for sessions**:
  * Specify the time since the last update (last time an authenticated API call used this session), after which the session will expire.
      * Please note that event polling requests (`/api/v1/event/poll`) are ignored. All other requests reset the expire time for sessions
  * Minimum duration: 1 minute (`'1m'`)
  * Default duration: 1 week (`'7d'`)

* **Purge expired sessions from the database**:
  * Enable or disable whether the janitor should delete all expired sessions from the database.

### Settings for deleting exports and downloads

* **Expiration time for exports and downloads**:
  * Specify after which time export and download folders should be deleted from the system.
  * Minimum duration: 1 minute (`'1m'`)
  * Default duration: 1 week (`'7d'`)

### Settings for deleting events

* **Expiration time for events**:
  * Specify after which time events in the list are being deleted.
    * Following events are deleted: `OBJECT_INDEX`, `API_PROGRESS`, `SUGGEST_INDEX_DONE`, `SUGGEST_INDEX_FAILED`, `EMAIL_SENT`
  * Minimum duration: 1 minute (`'1m'`)
  * Default duration: 1 week (`'7d'`)