# HA Line Notify
Line Notify custom component for Home Assistant.

Line is a messaging application widely used in Asia. It has notification service called "Line Notify" which you can use their API to send messages and media to your Line account. This integration let you send message, image and sticker to your Line account with Home Assistant.

## Installation
 1. Add This Repository to HACS as Integration.
 2. Download Line Notify.

OR

 1. Copy `line_notify` folder from `custom_components` to your custom_components in Home Assistant directory.

Then

 1. Reboot your Home Assistant instance.
 2. Go To `Settings -> Devices & Services -> [+ ADD INTEGRATION]` and do search "Line Notify".
 3. Set the notification name and access token and finish.
 4. Call `notify.(Notification name as specified in the settings)`(with `service parameters and data` described below) from script or automation as you desire.

## Supported parameters
Service data can be added in order to send message, image and sticker.

| Key            | Example value                                                                            | Description                        |
|:---------------|:-----------------------------------------------------------------------------------------|:-----------------------------------|
| `message `     | `Hello`                                                                                  | Message to be sent out to recipient|
| `data `        | `{"url":"https://picsum.photos/600/400","access_token":"xxxxxxxxxxxxxxxxxxxxxxxxxxxxx"}` | data to be send to line            |

## Supported data
Service data can be added in order to send message, image and sticker.

| Key            | Example value                   | Description                   |
|:---------------|:--------------------------------|:------------------------------|
| `access_token` | `xxxxxxxxxxxxxxxxxxxxxxxxxxxxx` | Access Token From Line Notify [Obtain Line Notify personal token.](https://notify-bot.line.me/en/) |
| `url`          | `https://picsum.photos/600/400` | URL of image file             |
| `file`         | `/config/tmp/test.jpg`          | Directory of image file       |
| `stkpkgid`     | `1`                             | Sticker package ID            |
| `stkid`        | `2`                             | Sticker ID                    |
| `notification_disabled`        | `false`                             | if set true doesn't receive a push notification when the message is sent.|

In order to send sticker, `stkpkgid` and `stkid` must be used together. List of sticker package ID and Sticker ID can be found [here](https://developers.line.biz/en/docs/messaging-api/sticker-list/).

JPG and PNG image format are support, but you have to either choose to send with a file or url per round. 


## Example
**Test call with service developer tools**
![test call](https://raw.githubusercontent.com/maxmacstn/HA-Line-Notify/master/sample_show.png)


**Send Camera Snapshot to Line when something was triggered.**

configuration.yaml (Optional)
```
homeassistant:
  whitelist_external_dirs:
    - /tmp
 ```
Note: You need to add whitelist directory in order to save camera image snapshot. (if you use /media folder, don't set that.)


script.yaml
```
'send_line_notify':
  alias: Send Line notify
  sequence:
 - data:
      filename: /tmp/snapshot.jpg
    entity_id: camera.living_room
    service: camera.snapshot
 - delay: '2'
 - data:
      message: Snapshot from living room camera.
      data:
        file: /tmp/snapshot.jpg
    service: notify.(Notification name as specified in the settings)
```
## To-Do
 - Multi-user support : let each user input their own token upon service call.
 - Simple login: using OAuth to obtain token from Line Notify API.
