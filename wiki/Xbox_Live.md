---
title: Xbox Live
permalink: wiki/Xbox_Live/
layout: wiki
---

Xbox Live is an online multiplayer gaming and digital media delivery
service created and operated by Microsoft. It was first made available
to the Xbox system in November 2002.
([Wikipedia](https://en.wikipedia.org/wiki/Xbox_Live)) Xbox Live support
for the original Xbox ended in April 15, 2010.

The Xbox Live architecture consists of authentication servers,
matchmaking servers, and game servers.

### Authentication servers

Authentication and access to Xbox Live services is controlled using the
Kerberos protocol with a few proprietary customizations for the Xbox.

Kerberos Authentication Server: macs.xboxlive.com

| padata-type | description                                              |
|-------------|----------------------------------------------------------|
| 131         | ?                                                        |
| 204         | ?                                                        |
| 206         | Information about Xbox Version, Title, and Title version |

### Matchmaking servers

### Game servers

### XDK Functions

| function                                                                                                                          | description                                                                                                                                                                                                                                                            |
|-----------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| XOnlineGetUsers(XONLINE\_USER\* XBLAccountusers, DWORD\* numOfXBLiveAccounts)                                                     | The XOnlineGetUsers function will enumerate both the hard disk and any attached memory units looking for user accounts                                                                                                                                                 |
| XOnlineTaskClose(XONLINETASK\_HANDLE logonHandle)                                                                                 | Called to abort the authentication process.                                                                                                                                                                                                                            |
| XOnlineStartup( XONLINE\_STARTUP\_PARAMS\* )                                                                                      |                                                                                                                                                                                                                                                                        |
| XOnlineLogon(XONLINE\_USER\* XBLLoggedOnUsers, DWORD\* XBLservices, DWORD SERVICE\_COUNT, NULL, XONLINETASK\_HANDLE &logonHandle) | When a title calls XOnlineLogon to sign in, instead of blocking until the authentication completes, an asynchronous task handle is returned. As part of the authentication process a title must specify which services it will be using (XBLservices, SERVICE\_COUNT). |
| XOnlineTaskContinue(XONLINETASK\_HANDLE logonHandle)                                                                              | Called to check the status of XOnlineLogon. It will return XONLINETASK\_S\_RUNNING while the login process has not been completed.                                                                                                                                     |
| XOnlineLogonTaskGetResults(XONLINETASK\_HANDLE logonHandle)                                                                       | Will return XONLINE\_S\_LOGON\_CONNECTION\_ESTABLISHED when the task is successfully completed. Otherwise it will return an error code.                                                                                                                                |
| XOnlineGetLogonUsers()                                                                                                            | This returns a pointer to an array of XONLINE USER structures. This array is similar the XONLINE USER array we populated and passed into XOnlineLogon, but is updated with error status and permission flags for each user.                                            |
| XOnlineSetUserGuestNumber(dwUserFlags , 1)                                                                                        |                                                                                                                                                                                                                                                                        |
| XOnlineTitleUpdate(DWORD)                                                                                                         | The XOnlineTitleUpdate function will boot into an updater application, which performs the actual update                                                                                                                                                                |
| XOnlineGetServiceInfo(Service, ?)                                                                                                 | XOnlineGetServiceInfo returns the connection status for a service                                                                                                                                                                                                      |
| XOnlineNotificationSetState                                                                                                       |                                                                                                                                                                                                                                                                        |


