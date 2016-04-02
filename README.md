# Slackboard

Slackboard is a proxy server for Slack.

## Status

Slackboard is production ready.

## Features

 * `slackboard`
  * A proxy server for Slack
 * `slackboard-cli`
  * A client for `slackboard`
 * `slackboard-log`
  * A client like [`cronlog`](https://github.com/kazuho/kaztools/blob/master/cronlog) for `slackboard`

## Installation

```
go get -u github.com/kaneshin/slackboard/...
```

## Configuration

See [CONFIGURATION.md](https://github.com/kaneshin/slackboard/blob/master/CONFIGURATION.md) about details.

## Specification

See [SPEC.md](https://github.com/kaneshin/slackboard/blob/master/SPEC.md) about details.

## Run

```
slackboard -c conf/slackboard.toml
```

## Client for Slackboard

`slackboard-cli` is a client for `slackboard`. It reads `stdin` and sends a message to `slackboard`.

```
echo message | slackboard-cli -t test -s slackboard-host:29800
```

When `-t tagname` is given to `slackboard-cli`, `slackboard-cli` uses
[POST /notify](https://github.com/kaneshin/slackboard/blob/master/SPEC.md#post-notify).
In this case, `slackboard-cli` accepts the options such as `-l`, `-sync`, `-title`.

And you can send a message to Slack's channel directly from v0.5.0.

```
echo message | slackboard-cli -c random -s slackboard-host:29800
```

When `-c channelname` is given to `slackboard-cli`, `slackboard-cli` uses [POST /notify-directly](https://github.com/kaneshin/slackboard/blob/master/SPEC.md#post-notify-directly). In this case, `slackboard-cli` accepts the all options except `-t` and `-l`.

```
echo message | slackboard-cli -c random -s slackboard-host:29800 -u username -i :icon_emoji: -C #ff00ff
```

## Attachments

From v0.6.0, `slackboard` supports [Attachments](https://api.slack.com/docs/attachments) partially.
For example, `slackboard-cli` can append color for message.

```
echo information | slackboard-cli -t test -s slackboard-host:29800 -l info
echo warning     | slackboard-cli -t test -s slackboard-host:29800 -l warn
echo critical    | slackboard-cli -t test -s slackboard-host:29800 -l crit
```

![attachment-color](https://raw.githubusercontent.com/kaneshin/slackboard/master/img/attachments.png)

Or you can set color keywords or color-code of [Slack Attachments](https://api.slack.com/docs/attachments) with `-C` directly.

```
echo good    | slackboard-cli -c random -s slackboard-host:29800 -C good
echo bad     | slackboard-cli -c random -s slackboard-host:29800 -C bad
echo warning | slackboard-cli -c random -s slackboard-host:29800 -C warning
```

### Synchronous notification with slackboard-cli

From v0.3.0, `slackboard-cli` sends a notification-request to `slackboard` asynchronously by default.
If you want to send a notification-request to `slackboard` synchronously, you may add the option `-sync` to `slackboard-cli`.

```
echo message | slackboard-cli -t test -s slackboard-host:29800 -sync
```

## Notification with slackboard-log

`slackboard-log` is a client for `slackboard` also. `slackboard-log` is an utility like [`cronlog`](https://github.com/kazuho/kaztools/blob/master/cronlog).
It sends a notification to `slackboard` when the command after `--` failed.

```
slackboard-log -s 127.0.0.1:29800 -t test -- some-command
```

From `v0.5.0`, you can send a message to Slack's channel directly.

```
slackboard-log -s 127.0.0.1:29800 -c random -- some-command
```

## License

Copyright 2014-2016 Tatsuhiko Kubo


Licensed under the MIT License.
