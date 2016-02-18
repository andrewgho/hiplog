hiplog - archive HipChat private chats to local disk
====================================================

This program archives [HipChat](https://www.hipchat.com/) private chats 
to local disk. It requires no administrative access, just a regular
[API personal access token](https://groupon.hipchat.com/account/api)
used for accessing the 
[HipChat v2 API](https://www.hipchat.com/docs/apiv2/).

Usage
-----

Get an HipChat API personal access token with *View Messages* privileges:

* https://groupon.hipchat.com/account/api

Archive chats into JSON files in directory `hiplogs/alice`:

    $ hiplog -t $access_token alice
