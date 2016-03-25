# How iCloud works

Trying to understand the various api calls iCloud.com uses to get your data from apple servers. Hope someone can use this information to build better products/apps.

## Thoughts

Some thoughts before you see all the api calls

- All you need for any api call is a 9 digit dsid and a cookie called X-APPLE-WEBAUTH-TOKEN. The 9 digit dsid can be found in Keychain Access under iCloud or in the file ~/Library/Application Support/CloudDocs/account.1 or atleast in 100 other places in your mac.

- All post calls pass the dsid and session id as query strings. If you use any free internet service or anyone with WireShark can easily hack your iCloud account. WTF!

## iCloud.com website

### Authenticate the client

POST call with the following
https://setup.icloud.com/setup/ws/1/validate?clientBuildNumber=<webapp build>&clientId=<session id>

webapp build - 16BHotfix??
sample uid - 5DD?????-AC??-??ED-9???-FD685B??????

Reponse error of 421 indicates that the client is new. This would trigger an automated email upon successful login. If you successfully block this call to validate endpoint then no email would get triggered. How cool and stupid is that?

Sample email
======
Your Apple ID was used to sign in to iCloud via a web browser
======

NOTE to apple: POST should not have values as query strings!

### Login process

POST to url
https://setup.icloud.com/setup/ws/1/login?clientBuildNumber=<webapp build>&clientId=<session id>
JSON
{"apple_id":"<email>","password":"<password>","extended_login":false}

Sample response
```json
{
  "pcsServiceIdentitiesIncluded": true,
  "dsInfo": {
    "lastName": "LastName",
    "iCDPEnabled": false,
    "dsid": "<9 digit id>",
    "ironcadeMigrated": true,
    "locale": "en-value_value",
    "isManagedAppleID": false,
    "gilligan-invited": "true",
    "appleIdAliases": [
      "FirstName.LastName@me.com",
      "FirstName.LastName@icloud.com"
    ],
    "isPaidDeveloper": false,
    "notificationId": "",
    "primaryEmailVerified": true,
    "aDsID": "<advertiser id>",
    "locked": false,
    "hasICloudQualifyingDevice": true,
    "primaryEmail": "FirstName.LastName@email.com",
    "gilligan-enabled": "true",
    "fullName": "FirstName LastName",
    "languageCode": "en-value",
    "appleId": "FirstName.LastName@email.com",
    "firstName": "FirstName",
    "iCloudAppleIdAlias": "FirstName.LastName@icloud.com",
    "notesMigrated": true,
    "appleIdAlias": "FirstName.LastName@me.com",
    "brMigrated": true,
    "statusCode": 2
  },
  "hasMinimumDeviceForPhotosWeb": true,
  "iCDPEnabled": false,
  "webservices": {
    "reminders": {
      "url": "https://p31-remindersws.icloud.com:443",
      "status": "active"
    },
    "calendar": {
      "url": "https://p31-calendarws.icloud.com:443",
      "status": "active"
    },
    "docws": {
      "pcsRequired": true,
      "url": "https://p31-docws.icloud.com:443",
      "status": "active"
    },
    "settings": {
      "url": "https://p31-settingsws.icloud.com:443",
      "status": "active"
    },
    "notes": {
      "url": "https://p05-notesws.icloud.com:443",
      "status": "active"
    },
    "mail": {
      "url": "https://p05-mailws.icloud.com:443",
      "status": "active"
    },
    "ubiquity": {
      "url": "https://p31-ubiquityws.icloud.com:443",
      "status": "active"
    },
    "streams": {
      "url": "https://p31-streams.icloud.com:443",
      "status": "active"
    },
    "keyvalue": {
      "url": "https://p31-keyvalueservice.icloud.com:443",
      "status": "active"
    },
    "ckdatabasews": {
      "pcsRequired": true,
      "url": "https://p31-ckdatabasews.icloud.com:443",
      "status": "active"
    },
    "photosupload": {
      "pcsRequired": true,
      "url": "https://p31-uploadphotosws.icloud.com:443",
      "status": "active"
    },
    "photos": {
      "pcsRequired": true,
      "uploadUrl": "https://p31-uploadphotosws.icloud.com:443",
      "url": "https://p31-photosws.icloud.com:443",
      "status": "active"
    },
    "archivews": {
      "url": "https://p31-archivews.icloud.com:443",
      "status": "active"
    },
    "push": {
      "url": "https://p31-pushws.icloud.com:443",
      "status": "active"
    },
    "drivews": {
      "pcsRequired": true,
      "url": "https://p31-drivews.icloud.com:443",
      "status": "active"
    },
    "iwmb": {
      "url": "https://p31-iwmb.icloud.com:443",
      "status": "active"
    },
    "cksharews": {
      "url": "https://p31-ckshare.icloud.com:443",
      "status": "active"
    },
    "iworkexportws": {
      "url": "https://p31-iworkexportws.icloud.com:443",
      "status": "active"
    },
    "findme": {
      "url": "https://p31-fmipweb.icloud.com:443",
      "status": "active"
    },
    "iworkthumbnailws": {
      "url": "https://p31-iworkthumbnailws.icloud.com:443",
      "status": "active"
    },
    "fmf": {
      "url": "https://p31-fmfweb.icloud.com:443",
      "status": "active"
    },
    "contacts": {
      "url": "https://p31-contactsws.icloud.com:443",
      "status": "active"
    },
    "account": {
      "iCloudEnv": {
        "shortId": "p"
      },
      "url": "https://p31-setup.icloud.com:443",
      "status": "active"
    }
  },
  "pcsEnabled": true,
  "requestInfo": {
    "country": "value",
    "timeZone": "GMT",
    "region": "EN"
  },
  "appsOrder": [
    "mail",
    "contacts",
    "calendar",
    "photos",
    "iclouddrive",
    "notes2",
    "reminders",
    "pages",
    "numbers",
    "keynote",
    "newspublisher",
    "fmf",
    "find",
    "settings"
  ],
  "iCloudInfo": {
    "SafariBookmarksHasMigratedToCloudKit": false
  },
  "version": 1,
  "isExtendedLogin": false,
  "apps": {
    "calendar": {

    },
    "reminders": {

    },
    "keynote": {
      "isQualifiedForBeta": true
    },
    "settings": {
      "canLaunchWithOneFactor": true
    },
    "mail": {

    },
    "numbers": {
      "isQualifiedForBeta": true
    },
    "photos": {

    },
    "pages": {
      "isQualifiedForBeta": true
    },
    "find": {
      "canLaunchWithOneFactor": true
    },
    "notes2": {

    },
    "iclouddrive": {

    },
    "newspublisher": {
      "isHidden": true
    },
    "fmf": {

    },
    "contacts": {

    }
  }
}
```

### Get token to subscribe to push messages

POST (Again with query parameters!)
https://p31-pushws.icloud.com/getToken?attempt=1&clientBuildNumber=<webapp build>&clientId=<session id>&dsid=<9 digit id>

```json
{
  "pushTokenTTL": 43200,
  "webCourierURL": "https:\/\/webcourier.push.apple.com\/aps",
  "registeredTopics": [
    "<oid 40 characters>",
    "<oid 40 characters>",
    "<oid 40 characters>",
    "<oid 40 characters>",
    "<oid 40 characters>"
  ],
  "pushToken": "<oid 64 characters>"
}
```

### Get the current state to show any badges for the apps

POST https://p31-pushws.icloud.com/getState?clientBuildNumber=<webapp build>&clientId=<session id>&dsid=<9 digit id>&pcsEnabled=true

```json
{
  "states": [
    {
      "data": {
        "badge": 0
      },
      "pushTopic": "<oid 40 characters>"
    },
    {
      "data": {
        "badge": 0
      },
      "pushTopic": "<oid 40 characters>"
    },
    {
      "data": {
        "badge": 0
      },
      "pushTopic": "<oid 40 characters>"
    },
    {
      "data": {
        "badge": 0
      },
      "pushTopic": "<oid 40 characters>"
    }
  ]
}
```

### Get detailed settings about the user (sync the client view)

POST https://p31-keyvalueservice.icloud.com/json/sync?clientBuildNumber=<webapp build>&clientId=<session id>&dsid=<9 digit id>

```json
{
  "timestamp": 1458910490003,
  "status": 0,
  "apps": [
    {
      "registry-version": "FT=-@RU=<guid>@S=227000",
      "keys": [
        {
          "name": "lastUsedApp",
          "data": "mail"
        },
        {
          "name": "locale",
          "data": "en-us"
        },
        {
          "name": "regionFormat",
          "data": "en-value"
        },
        {
          "name": "regionFormatsMatchLanguage",
          "data": false
        },
        {
          "name": "startupUrls",
          "data": {
            "notes2": "{\"loadCoreTypes\":true,\"url\":\"https://p31-ckdatabasews.icloud.com/database/1/com.apple.notes/production/private/records/changes?\",\"method\":\"POST\",\"requestPayload\":{\"zoneID\":{\"zoneName\":\"Notes\"},\"desiredKeys\":[\"TitleEncrypted\",\"SnippetEncrypted\",\"FirstAttachmentUTIEncrypted\",\"FirstAttachmentThumbnail\",\"FirstAttachmentThumbnailOrientation\",\"ModificationDate\",\"Deleted\",\"Folders\",\"Attachments\"],\"desiredRecordTypes\":[\"Note\"],\"reverse\":true}}",
            "contacts": "https://p31-contactsws.icloud.com:443/co/startup?clientVersion=2.1&dsid=<9 digit id>&locale=en_US&order=first%2Clast"
          }
        },
        {
          "name": "timeZone",
          "data": "value"
        },
        {
          "name": "timeZoneCity",
          "data": "value"
        },
        {
          "name": "timezoneName",
          "data": "value"
        },
        {
          "name": "userPreferredAppleId",
          "data": "email"
        }
      ],
      "registry-status": 1,
      "app-id": "account"
    },
    {
      "registry-version": "FT=-@RU=<guid>@S=337",
      "keys": [
        {
          "name": "countryFormat",
          "data": "country code"
        },
        {
          "name": "displayFields",
          "data": "first,last"
        },
        {
          "name": "displayOrderBy",
          "data": "first,last"
        },
        {
          "name": "preferenceId",
          "data": "1"
        },
        {
          "name": "usePhoneFormat",
          "data": true
        }
      ],
      "registry-status": 1,
      "app-id": "contacts"
    },
    {
      "registry-version": "FT=-@RU=<guid>@S=225132",
      "keys": [
        {
          "name": "alarmsByDefault",
          "data": true
        },
        {
          "name": "calendarListWidth",
          "data": 250
        },
        {
          "name": "customRemindersListWidth",
          "data": 0
        },
        {
          "name": "dateFormat",
          "data": "DD/MM/YYYY"
        },
        {
          "name": "dateSeparator",
          "data": "/"
        },
        {
          "name": "defaultCalendar",
          "data": "<cal id>"
        },
        {
          "name": "defaultEventAlarm",
          "data": {
            "minutes": 15,
            "days": 0,
            "seconds": 0,
            "before": true,
            "hours": 0,
            "weeks": 0
          }
        },
        {
          "name": "emailParticipantsSharingChange",
          "data": false
        },
        {
          "name": "enableTimeZone",
          "data": true
        },
        {
          "name": "hideEvents",
          "data": true
        },
        {
          "name": "hideEventsNum",
          "data": 10
        },
        {
          "name": "hideToDos",
          "data": true
        },
        {
          "name": "hideToDosNum",
          "data": 7
        },
        {
          "name": "hideToDosOutsideView",
          "data": true
        },
        {
          "name": "isCalendarListVisible",
          "data": true
        },
        {
          "name": "lastAlarmSet",
          "data": [

          ]
        },
        {
          "name": "lastTimeZoneSet",
          "data": [
            "value",
            "value"
          ]
        },
        {
          "name": "promptEventUpdates",
          "data": false
        },
        {
          "name": "saveToMailbox",
          "data": false
        },
        {
          "name": "selectedCollection",
          "data": "home"
        },
        {
          "name": "showBirthdays",
          "data": false
        },
        {
          "name": "showDeclinedEvents",
          "data": false
        },
        {
          "name": "showHoursAtATime",
          "data": 8
        },
        {
          "name": "showLunarCalendar",
          "data": false
        },
        {
          "name": "showTodoPane",
          "data": true
        },
        {
          "name": "startWithTodayInWeekView",
          "data": false
        },
        {
          "name": "startingWeekday",
          "data": 0
        },
        {
          "name": "timeFormat",
          "data": 12
        },
        {
          "name": "timeSeparator",
          "data": ":"
        },
        {
          "name": "todoSortOrder",
          "data": "priority"
        },
        {
          "name": "triggerAlarmsInUI",
          "data": false
        },
        {
          "name": "viewingMode",
          "data": 2
        },
        {
          "name": "visibleDays",
          "data": 5
        }
      ],
      "registry-status": 1,
      "app-id": "calendar"
    },
    {
      "registry-version": "",
      "keys": [

      ],
      "registry-status": 1,
      "app-id": "find"
    },
    {
      "registry-version": "",
      "keys": [

      ],
      "registry-status": 1,
      "app-id": "fmf"
    },
    {
      "registry-version": "FT=-@RU=<guid>@S=226965",
      "keys": [
        {
          "name": "alias:alias",
          "data": {
            "guid": "alias:alias",
            "emailId": "alias",
            "color": "#a8a8a8",
            "active": true,
            "fullName": "",
            "emails": [
              {
                "address": "alias@me.com",
                "canSendFrom": false
              },
              {
                "address": "alias@icloud.com",
                "canSendFrom": false
              }
            ],
            "type": "CoreMail.AliasAccount",
            "accountDescription": ""
          }
        },
        {
          "name": "country",
          "data": "value"
        },
        {
          "name": "delMsgFolder",
          "data": "folder:Deleted Messages"
        },
        {
          "name": "deleteMessageAfterForwarding",
          "data": false
        },
        {
          "name": "enableVacationReply",
          "data": false
        },
        {
          "name": "forwardEmail",
          "data": false
        },
        {
          "name": "forwardEmailAddress",
          "data": ""
        },
        {
          "name": "identityAccountGuid",
          "data": {
            "guid": "identityAccountGuid",
            "emailId": "firstname.lastname",
            "fullName": "Prabhu Subramanian",
            "emails": [
              {
                "address": "firstname.lastname@me.com",
                "canSendFrom": true
              },
              {
                "address": "firstname.lastname@icloud.com",
                "canSendFrom": false
              }
            ],
            "type": "CoreMail.IdentityAccount",
            "accountDescription": ""
          }
        },
        {
          "name": "moveTrash",
          "data": true
        },
        {
          "name": "timeZone",
          "data": "value"
        },
        {
          "name": "tzOffset",
          "data": 0
        },
        {
          "name": "vacationReply",
          "data": ""
        },
        {
          "name": "~notifyTable",
          "data": [
            {
              "host": "gateway.push.apple.com",
              "token": "<64 character>",
              "timeout": 1452814578
            }
          ]
        }
      ],
      "registry-status": 1,
      "app-id": "mail"
    },
    {
      "registry-version": "",
      "keys": [

      ],
      "registry-status": 1,
      "app-id": "notes2"
    },
    {
      "registry-version": "",
      "keys": [

      ],
      "registry-status": 1,
      "app-id": "reminders"
    },
    {
      "registry-version": "",
      "keys": [

      ],
      "registry-status": 1,
      "app-id": "photos"
    },
    {
      "registry-version": "",
      "keys": [

      ],
      "registry-status": 1,
      "app-id": "iclouddrive"
    },
    {
      "registry-version": "",
      "keys": [

      ],
      "registry-status": 1,
      "app-id": "settings"
    },
    {
      "registry-version": "",
      "keys": [

      ],
      "registry-status": 1,
      "app-id": "pages"
    },
    {
      "registry-version": "FT=-@RU=<guid>@S=70168",
      "keys": [
        {
          "name": "didHideTags",
          "data": true
        },
        {
          "name": "didShowWelcome",
          "data": true
        }
      ],
      "registry-status": 1,
      "app-id": "numbers"
    },
    {
      "registry-version": "",
      "keys": [

      ],
      "registry-status": 1,
      "app-id": "keynote"
    },
    {
      "registry-version": "",
      "keys": [

      ],
      "registry-status": 1,
      "app-id": "newspublisher"
    }
  ]
}
```

### Get storage info

POST https://setup.icloud.com/setup/ws/1/storageUsageInfo?clientBuildNumber=<webapp build>&clientId=<session id>&dsid=<9 digit id>

```json
{
  "storageUsageByMedia": [
    {
      "displayColor": "98d1fd",
      "usageInBytes": 0,
      "displayLabel": "Photos and Videos",
      "mediaKey": "photos"
    },
    {
      "displayColor": "fef236",
      "usageInBytes": 0,
      "displayLabel": "Backup",
      "mediaKey": "backup"
    },
    {
      "displayColor": "bdf131",
      "usageInBytes": 0,
      "displayLabel": "Docs",
      "mediaKey": "docs"
    },
    {
      "displayColor": "fec92e",
      "usageInBytes": 0,
      "displayLabel": "Mail",
      "mediaKey": "mail"
    }
  ],
  "storageUsageInfo": {
    "compStorageInBytes": 0,
    "usedStorageInBytes": 0,
    "totalStorageInBytes": 0,
    "commerceStorageInBytes": 0
  },
  "quotaStatus": {
    "overQuota": false,
    "haveMaxQuotaTier": false,
    "paidQuota": false
  }
}
```

### Get me card
POST https://p31-contactsws.icloud.com/co/mecard/?clientBuildNumber=<webapp build>&clientId=<session id>&dsid=<9 digit id>

```json
{
  "meCardId": "<48 char id>",
  "contacts": [
    {
      "lastName": "",
      "notes": "",
      "contactId": "<48 char id>",
      "nickName": "",
      "prefix": "",
      "relatedNames": [
        {
          "field": "",
          "label": "SPOUSE"
        },
        {
          "field": "",
          "label": "CHILD"
        },
        {
          "field": "",
          "label": "MOTHER"
        }
      ],
      "companyName": "",
      "photo": {
        "url": "<url requiring X-APPLE-WEBAUTH-TOKEN cookie>"
      },
      "phones": [
        {
          "field": "",
          "label": "MOBILE"
        }
      ],
      "isCompany": false,
      "suffix": "",
      "IMs": [
        {
          "field": {
            "IMService": "Jabber",
            "userName": ""
          },
          "label": "HOME"
        }
      ],
      "firstName": "",
      "emailAddresses": [
        {
          "field": "",
          "label": "HOME"
        },
        {
          "field": "",
          "label": "WORK"
        }
      ],
      "etag": "",
      "middleName": ""
    }
  ]
}
```

### Get calendar alarms

POST https://p31-calendarws.icloud.com/ca/alarmtriggers/?clientBuildNumber=<webapp build>&clientId=<session id>&dsid=<9 digit id>&endDate=YYYY-M-DD&lang=en-??&startDate=YYYY-M-DD&usertz=<timezone>

```json
{
  "AlarmTrigger": [
    {
      "guid": "",
      "triggerDate": [
        20160326,
        2016,
        3,
        26,
        9,
        0,
        540
      ],
      "messageType": "message",
      "pGuid": "",
      "title": "Easter Monday",
      "location": null,
      "startDate": [
        20160328,
        2016,
        3,
        28,
        0,
        0,
        0
      ],
      "endDate": [
        20160329,
        2016,
        3,
        29,
        0,
        0,
        0
      ],
      "allDay": true,
      "tz": null,
      "duration": 1440,
      "localStartDate": [
        20160328,
        2016,
        3,
        28,
        0,
        0,
        0
      ],
      "localEndDate": [
        20160329,
        2016,
        3,
        29,
        0,
        0,
        0
      ]
    }
  ]
}
```

### Get push messages

- Use the token received from get token call
- Call and wait https://webcourier.push.apple.com/aps?tok=<oid 64 characters>&ttl=43200

### Periodic heartbeat to keep session alive

GET https://p31-pushws.icloud.com/refreshWebAuth?clientBuildNumber=<webapp build>&clientId=<session id>&dsid=<9 digit id>

```json
{}
```

### Statistics collection

Or we don't collect any data about our users marketing crap.

Tip: Block feedbackws.icloud.com in /etc/hosts

POST https://feedbackws.icloud.com/reportStats

Request json
```json
{
  "stats": [
    {
      "httpMethod": "POST",
      "statusCode": 200,
      "hostname": "www.icloud.com",
      "urlPath": "/setup/ws/1/validate",
      "clientTiming": 1956,
      "uncompressedResponseSize": 3464,
      "region": "EN",
      "country": "GB",
      "statName": "cloudosRequestInfo",
      "sessionID": "<session id>",
      "appName": "cloudos",
      "isLiteAccount": false
    },
    {
      "httpMethod": "POST",
      "statusCode": 200,
      "hostname": "www.icloud.com",
      "urlPath": "/setup/ws/1/storageUsageInfo",
      "clientTiming": 275,
      "uncompressedResponseSize": 603,
      "region": "EN",
      "country": "GB",
      "statName": "cloudosRequestInfo",
      "sessionID": "<session id>",
      "appName": "cloudos",
      "isLiteAccount": false
    },
    {
      "httpMethod": "POST",
      "statusCode": 200,
      "hostname": "www.icloud.com",
      "urlPath": "/registerTopics",
      "clientTiming": 583,
      "uncompressedResponseSize": 439,
      "region": "EN",
      "country": "GB",
      "statName": "cloudosRequestInfo",
      "sessionID": "<session id>",
      "appName": "cloudos",
      "isLiteAccount": false
    },
    {
      "httpMethod": "GET",
      "statusCode": 200,
      "hostname": "www.icloud.com",
      "urlPath": "/co/mecard/",
      "clientTiming": 645,
      "uncompressedResponseSize": 1213,
      "region": "EN",
      "country": "GB",
      "statName": "cloudosRequestInfo",
      "sessionID": "<session id>",
      "appName": "cloudos",
      "isLiteAccount": false
    },
    {
      "asset": "https://www.icloud.com/blank.png",
      "loadTime": 206,
      "region": "EN",
      "country": "GB",
      "statName": "cloudosEdgeLoad",
      "sessionID": "<session id>",
      "hostname": "www.icloud.com",
      "appName": "cloudos",
      "isLiteAccount": false
    },
    {
      "asset": "https://icloud.com/blank.png",
      "loadTime": 627,
      "region": "EN",
      "country": "GB",
      "statName": "cloudosOriginLoad",
      "sessionID": "<session id>",
      "hostname": "www.icloud.com",
      "appName": "cloudos",
      "isLiteAccount": false
    }
  ]
}
```
