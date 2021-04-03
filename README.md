# OctoRave

A tweak on [OctoRave](https://github.com/bchanudet/OctoPrint-OctoRave) with a few minor tweaks to fit my particular use case. An Octoprint plugin allowing Octoprint to send notifications to a Discord channel via a webhook URL.

## Setup

### Install the plugin

Install via the bundled [Plugin Manager](https://github.com/foosel/OctoPrint/wiki/Plugin:-Plugin-Manager)
or manually using this URL:

    https://github.com/alinsavix/OctoPrint-OctoRave/archive/master.zip


### Create the WebHook in Discord

*Note : you need to have the permission "Manage WebHooks" to create or edit a WebHook in Discord.*

1. Go to the channel settings

1. Under "Webhooks", click "Create Webhook"

1. Enter the details, and copy the URL at the bottom

Once you got the WebHook URL, head over to the plugin configuration to finish the setup.


## Configuration

The plugin can be configured in the configuration panel, under the "OctoRave" panel.


### Discord Settings

- WebHook URL : please follow the Setup procedure to retrieve the URL of the WebHook.
- Bot name : You can override the name you put on the WebHook description in Discord by a name. Useful if the webhook is not specific to OctoRave and also used for other things.
- Bot Avatar URL : You can also override the avatar us put in Discord for the WebHook. The URL needs to be globally accessible (it will be retrieved by Discord's servers).

In order for you to be sure these settings work, every time you change one of them, a test message will be sent to the corresponding Discord Channel. If you don't receive it, something is most likely wrong!


### Message Settings

Here you can customize every message handled by OctoRave.

- **Toggle the message** : by unchecking the checkbox in front of the message title, you can disable the message. It won't be sent to Discord.
- **Message** : you can change the default content here. See the section [Message format](#message-format) for more information.
- **Include snapshot** : if you have a snapshot URL defined in the Octoprint settings, you can choose to upload a snapshot with the message to Discord.
- **Notify every `XX`%** : specific to the `printing progress` message, this settings allows you to change the frequency of the notification :
    - `10%` means you'll receive a message at 10%, 20%, 30%, 40% ... 80%, 90% of the printing process.
    - `5%` means you'll receive a message at 5%, 10%, 15%, 20% ... 80%, 85%, 90%, 95% of the printing process.
    - etc...


### Scripts Settings

OctoRave allows you to launch scripts every time a message is sent:

- Before sending: perfect for turning some LED on to ensure the webcam will always have enough light when taking the snapshot
- After sending: perfect for turning the same LED off :)

Script configuration was made voluntarily a little harder, as running scripts exposes much more the host computer.


## Message format

Messages are regular Discord messages, which means you can use :
- `**markdown**` format (see [Discord Documentation](https://support.discordapp.com/hc/en-us/articles/210298617-Markdown-Text-101-Chat-Formatting-Bold-Italic-Underline-))
- `:emoji:` shortcuts to display emojis
- `@mentions` to notify someone

Some events also support variables, here is a basic list :

**Printing process : started event** :
- `{name}` : file's name that's being printed
- `{path}` : file's path within its origin location
- `{origin}` : the origin storage location

**Printing process : failed event** :
- `{name}` : file's name that's being printed
- `{path}` : file's path within its origin location
- `{origin}` : the origin storage location

**Printing process : done event** :
- `{name}` : file's name that's being printed
- `{path}` : file's path within its origin location
- `{origin}` : the origin storage location
- `{time}`: time needed for the print (in seconds)
- `{time_formatted}` : same as `{time}`, but in a human-readable format (`HH:MM:SS`)

**Printing process : failed event** :
- `{name}` : file's name that's being printed
- `{path}` : file's path within its origin location
- `{origin}` : the origin storage location
- `{position}`: position of the hotend

**Printing process : paused event** :
- `{name}` : file's name that's being printed
- `{path}` : file's path within its origin location
- `{origin}` : the origin storage location
- `{position}`: position of the hotend

**Printing process : resumed event** :
- `{name}` : file's name that's being printed
- `{path}` : file's path within its origin location
- `{origin}` : the origin storage location
- `{position}`: position of the hotend

**Printing progress event** :
- `{progress}` : progress in % of the print.
- `{spent}`: time spent since the start of the print (in seconds)
- `{spent_formatted}` : same as `{spent}`, but in a human-readable format (`HH:MM:SS`)
- `{remaining}`: time remaining until the end of the print (in seconds)
- `{remaining_formatted}` : same as `{remaining}`, but in a human-readable format (`HH:MM:SS`)

**Printer state : error**
- `{error}` : The error received

For more reference, you can go to the [Octoprint documentation on Events](http://docs.octoprint.org/en/master/events/index.html#sec-events-available-events)
