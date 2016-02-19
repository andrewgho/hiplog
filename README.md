hiplog - archive HipChat private chats to local disk
====================================================

The `hiplog` program archives [HipChat](https://www.hipchat.com/)
private chats to data files on local disk. Both the chat content and any
uploaded files or images are archived. One example of why you might wish
to do this is if a co-worker is about to leave your company, but you
wish to archive your chats with them before their HipChat account is
shut down.

Usage
-----

Get an HipChat API personal access token with *View Messages* privileges:

* https://www.hipchat.com/account/api

Install the `hiplog` program into any directory in your `$PATH`. You
will need any version of Ruby, and the `json` RubyGem.

    $ cp hiplog ~/bin

Archive chats with co-workers Alice and Bob into JSON files in directory
`hiplogs/alice` and `hiplogs/bob`, respectively:

    $ hiplog -t $access_token alice@example.com bob@example.com

Description
-----------

`hiplog` uses the [HipChat v2 API](https://www.hipchat.com/docs/apiv2/)
to connect to HipChat and download chat content and uploaded files or
images to JSON data files on local disk. By default, it will try to
download all available private chat history with a particular user,
back to the beginning of when the two of you began chatting.

The supported mechanism for
[exporting chat history](https://confluence.atlassian.com/display/HIPCHATKB/Exporting+chat+history)
from HipChat requires administrative access. In contrast, `hiplog` uses a
[personal API access token](https://www.hipchat.com/account/api).
This means that administrative access is not required, but it also means
that only private chats which are still accessible to your regular
HipChat mobile or desktop client can be archived. You cannot, for
example, get chat history for someone whose account has been deleted,
or get chat history for a user other than yourself.

### API Access Token

To use `hiplog`, you will need to
[generate a personal API access token](https://www.hipchat.com/account/api).
Log in using your usual HipChat credentials, then create a new access
token with at least *View Messages* privileges.

An access token is typically a 40-character alphanumeric string. Pass
the access token to `hiplog` using any of the following mechanisms, in
order of descending precedence:

* Pass via command line with the `-t` (`--token`) option
* Set as value of a `HIPCHAT_ACCESS_TOKEN` environment variable
* Save it to the file `~/.hipchat_access_token`

Note that your access token is as good as a password, so you may wish to
secure it accordingly (for example, by keeping restrictive permissions
on your `.hipchat_access_token` file).

### Installation

`hiplog` requires Ruby, but any version should work—it has been tested
with versions from REE (Ruby 1.8.7) to Ruby 2.2.4. The only dependency
which might not be already present in the Ruby standard library is the
`json` RubyGem. You can install this the normal way:

    $ gem install json

Or, you can install it with [Bundler](http://bundler.io/):

    $ bundle install

To run `hiplog`, just copy it to any directory in your `$PATH`. Or, just
directly run `hiplog` from wherever you saved it.

### Command Line Options

`hiplog` recognizes the following command line options:

* `-h`, `--help`

  Display command line help, then exit.

* `-v`, `--verbose`

  While running, print URLs and output filenames to *stderr*.
  The default is to run silently unless there is an error.

* `-t`, `--token`

  Use this HipChat API access token. The default is to try to read
  an access token from the `HIPCHAT_ACCESS_TOKEN` environment variable,
  or the file `~/.hipchat_access_token`, respectively.

* `-d`, `--output-dir`

  Write logs into subdirectories of this output directory.
  The default is a `hiplogs` directory in the current directory.
  The output directory will be created if it does not already exist.

Any other non-option arguments to `hiplog` are assumed to be HipChat
user IDs (which may be usernames, like `alice@example.com`, or numeric
user IDs, obtained via your own HipChat API calls.

### Archived Files

By default, `hiplog` will save chat history to the directory `hiplogs`
in the current directory. If that directory does not exist, it will be
created with default permissions. Logs will be saved to directories
underneath it named by username, then in JSON files named by the date
of the earliest item in that file:

    hiplogs
     ├──alice@example.com
     │  ├──20140919T014318.json
     │  └──20151013T205334.json
     └──bob@example.com
        ├──20150324T230614.json
        ├──20150324T230902.json
        ├──20150812T215405.json
        ├──20151005T220915.json
        ├──ZajifYh5KDgxtmS9i38K1A.upload.png
        └──AlMb_11bTp24W1vY6z-8mA.IMG_0729.JPG

The datestamped JSON files contain chat logs. Attachments are saved with
the UUID from their original message, encoded via Base64 for URLs, and
with their original filenames appended.

### Exit Status

`hiplog` will return zero on success, non-zero with the number of errors
recorded if there are errors.

Author
------

Andrew Ho (<andrew@zeuscat.com>)

License
-------

HipChat is a trademark of Atlassian Pty Ltd, and the use of that name in
this documentation does not imply any affiliation with or endorsement by
Atlassian Pty Ltd. The files in this repository are authored by Andrew
Ho and are covered by the following 3-clause BSD license:

    Copyright (c) 2016, Andrew Ho.
    All rights reserved.

    Redistribution and use in source and binary forms, with or without
    modification, are permitted provided that the following conditions
    are met:

    Redistributions of source code must retain the above copyright notice,
    this list of conditions and the following disclaimer.

    Redistributions in binary form must reproduce the above copyright
    notice, this list of conditions and the following disclaimer in the
    documentation and/or other materials provided with the distribution.

    Neither the name of the author nor the names of its contributors may
    be used to endorse or promote products derived from this software
    without specific prior written permission.

    THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
    "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
    LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR
    A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT
    HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
    SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT
    LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE,
    DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY
    THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
    (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
    OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
