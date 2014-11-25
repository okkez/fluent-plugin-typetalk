# fluent-plugin-typetalk [![Build Status](https://travis-ci.org/nulab/fluent-plugin-typetalk.svg?branch=master)](https://travis-ci.org/nulab/fluent-plugin-typetalk)

## Overview

[Fluentd](http://fluentd.org) plugin to emit notifications to [Typetalk](http://typetalk.in/).

## Installation

Install with gem or fluent-gem command as:

```
# for fluentd
$ gem install fluent-plugin-typetalk

# for td-agent
$ sudo /usr/lib64/fluent/ruby/bin/fluent-gem install fluent-plugin-typetalk
```

## Configuration

### Usage

This plugin uses client credentials for authentication. See [the developer document](http://developers.typetalk.in/oauth.html) how to get your own credential.
```
<match ...>
  type typetalk
  client_id YOUR_CLIENT_ID
  client_secret YOUR_CLIENT_SECRET
  topic_id YOUR_TOPIC_ID
  message notice: %s [%s] %s errors found
  out_keys tag,time,count
  time_key time
  time_format %Y/%m/%d %H:%M:%S
  tag_key tag
</match>
```

The notified message will look like this : `notice: count.service [2014/05/13 03:34:02] 200 errors found`. The meanings of out_keys,time_key,time_format and tag_key are equivalent to those of [fluent-plugin-ikachan](https://github.com/tagomoris/fluent-plugin-ikachan).

This plugin allows you to use special value namely $hostname in out_keys. By using it, you can include hostname within the notified message.
```
<match ...>
  type typetalk
  client_id YOUR_CLIENT_ID
  client_secret YOUR_CLIENT_SECRET
  topic_id YOUR_TOPIC_ID
  message notice: %s [%s] %s errors found on %s
  out_keys tag,time,count,$hostname
  time_key time
  time_format %Y/%m/%d %H:%M:%S
  tag_key tag
</match>
```

By default, the number of notification you can send within a minute is limited to 10. Log lines that exceed this limit will be discarded. You can change it with `interval` and `limit` parameters.
```
<match ...>
  type typetalk
  :
  interval 300
  limit 10
</match>
```

In the example above, you can allow 10 notifications to be sent within 300 seconds. To remove the notification throttle, set both parameters to 0.

## TODO

Pull requests are very welcome!!

## For developers

You have to run the command below when starting development.
```
$ bundle install --path vendor/bundle
```

To run tests, do the following.
```
$ VERBOSE=1 bundle exec rake test
```

When releasing, call rake release as follows.
```
$ bundle exec rake release
```

## Copyright

* Copyright (c) 2014- Takashi Someda ([@tksmd](http://twitter.com/tksmd/))
* Apache License, Version 2.0
