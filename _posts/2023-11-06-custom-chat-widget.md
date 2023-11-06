---
layout: post
title: "Creating A Custom Chat Widget In StreamElements (Step By Step Tutorial)"
subtitle: "Ever wanted to create a chat widget for your stream, but don't know where to start? I'll help you to create your own custom chat widget in StreamElements with this step by step tutorial."
background: "https://images.unsplash.com/photo-1498050108023-c5249f4df085?auto=format&fit=crop&q=80&w=2672"
categories: [streaming]
keywords:
  [
    twitch update,
    twitch advertisement,
    twitch terms of service,
    twitch multistreaming,
    twitch exclusivity,
  ]
---

<img src="https://images.unsplash.com/photo-1498050108023-c5249f4df085?auto=format&fit=crop&q=80&w=2672" alt="Creating A Custom Chat Widget In StreamElements">

<br>

**Jump to section**:

- [Introduction](#introduction)
- [Getting The Base Code In StreamElements](#getting-the-base-code-in-streamelements)
  - [Add the base HTML](#add-the-base-html)
  - [Add the base CSS](#add-the-base-css)
  - [Add the base JS](#add-the-base-js)
  - [Add the base FIELDS](#add-the-base-fields)
- [Customizing Your Widget In StreamElements](#customizing-your-widget-in-streamelements)
  - [Add the custom FIELDS](#add-the-custom-fields)
  - [Add the custom CSS](#add-the-custom-css)
  - [Add the custom DATA](#add-the-custom-data)
- [Backing Up Your Code](#backing-up-your-code)
  - [Organize your code](#organize-your-code)
  - [Upload your code to GitHub](#upload-your-code-to-github)
- [Sharing And Selling Your StreamElements Widgets](#sharing-and-selling-your-streamelements-widgets)
  - [Share some text files](#share-some-text-files)
  - [Share your widget in the StreamElements Discord](#share-your-widget-in-the-streamelements-discord)
  - [Apply for StreamElements' Overlay Sharing Application](#apply-for-streamelements-overlay-sharing-application)
- [To Wrap Up](#to-wrap-up)

<br>

## Introduction

Creating your own chat widget for your Twitch stream is a fulfilling endeavor. It allows you to add your own flair and customization to your stream.

However, in my journey of creating a custom StreamElements widget, I found a distinct lack of guided tutorials to do so. So I wanted to share what I've learned and how I created a custom chat widget in StreamElements.

If you're interested in creating your own chat widget, feel free to follow along and modify the code as you see fit.

Let's get started.

<br>

## Getting The Base Code In StreamElements

Open up your [StreamElements Overlays Dashboard](https://streamelements.com/dashboard/overlays).

Click on "New overlay" to create a new overlay.

Give the overlay a new name, like "Custom Chat".

Expand Settings and click "Open editor".

Navigate to https://github.com/StreamElements/widgets/tree/master/CustomChat.

### Add the base HTML

Copy the code from [StreamElements' custom chat widget.html](https://github.com/StreamElements/widgets/blob/master/CustomChat/widget.html) and paste it in the HTML section.

It should look like this:

```html
<script src="//cdnjs.cloudflare.com/ajax/libs/blueimp-md5/2.12.0/js/md5.min.js"></script>
<div class="main-container"></div>
```

### Add the base CSS

Copy the code from [StreamElements' custom chat widget.css](https://github.com/StreamElements/widgets/blob/master/CustomChat/widget.css) and paste it in the CSS section.

It should look like this:

```css
@import url('https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.5.2/animate.min.css');
@import url('https://fonts.googleapis.com/css?family={{fontName}}');

* {
    font-family: '{{fontName}}', sans-serif;
    color: {{fontColor}};
    overflow: hidden;
    font-weight: {{fontWeight}};
    text-shadow:{textShadow};
}

.main-container{
    display:{{alignMessages}};
    flex-direction: column;
    justify-content: flex-end;
    align-items: flex-start;
    align-content: flex-start;
    height:98%;
    margin-bottom:10px;
    box-sizing: border-box;
}

.message-row{
    flex: 0 0 auto;
    width:100%;
    margin-bottom:5px;
    background-color:{{backgroundColor}};
    padding:5px;
    vertical-align: baseline;
}

.badge{
    display:{{displayBadges}};
    height:{{fontSize}}px;
}

.user-box{
    display:inline;
    font-size:{{fontSize}}px;
}

.user-box > span{
    font-size:{{fontSize}}px;
}

.user-message{
    display:inline;
    font-size:{{fontSize}}px;
    word-wrap: break-word;
}

.emote{
    height: {emoteSize}px;
    vertical-align: middle;
    background-repeat:no-repeat;
}

.action{
    font-style: italic;
}
```

### Add the base JS

Copy the code from [StreamElements' custom chat widget.js](https://github.com/StreamElements/widgets/blob/master/CustomChat/widget.js) and paste it in the JS section.

It should look like this:

```js
let totalMessages = 0,
  messagesLimit = 0,
  nickColor = "user",
  removeSelector,
  addition,
  customNickColor,
  channelName,
  provider;
let animationIn = "bounceIn";
let animationOut = "bounceOut";
let hideAfter = 60;
let hideCommands = "no";
let ignoredUsers = [];
window.addEventListener("onEventReceived", function (obj) {
  if (obj.detail.event.listener === "widget-button") {
    if (obj.detail.event.field === "testMessage") {
      let emulated = new CustomEvent("onEventReceived", {
        detail: {
          listener: "message",
          event: {
            service: "twitch",
            data: {
              time: Date.now(),
              tags: {
                "badge-info": "",
                badges: "moderator/1,partner/1",
                color: "#5B99FF",
                "display-name": "StreamElements",
                emotes: "25:46-50",
                flags: "",
                id: "43285909-412c-4eee-b80d-89f72ba53142",
                mod: "1",
                "room-id": "85827806",
                subscriber: "0",
                "tmi-sent-ts": "1579444549265",
                turbo: "0",
                "user-id": "100135110",
                "user-type": "mod",
              },
              nick: channelName,
              userId: "100135110",
              displayName: channelName,
              displayColor: "#5B99FF",
              badges: [
                {
                  type: "moderator",
                  version: "1",
                  url: "https://static-cdn.jtvnw.net/badges/v1/3267646d-33f0-4b17-b3df-f923a41db1d0/3",
                  description: "Moderator",
                },
                {
                  type: "partner",
                  version: "1",
                  url: "https://static-cdn.jtvnw.net/badges/v1/d12a2e27-16f6-41d0-ab77-b780518f00a3/3",
                  description: "Verified",
                },
              ],
              channel: channelName,
              text: "Howdy! My name is Bill and I am here to serve Kappa",
              isAction: !1,
              emotes: [
                {
                  type: "twitch",
                  name: "Kappa",
                  id: "25",
                  gif: !1,
                  urls: {
                    1: "https://static-cdn.jtvnw.net/emoticons/v1/25/1.0",
                    2: "https://static-cdn.jtvnw.net/emoticons/v1/25/1.0",
                    4: "https://static-cdn.jtvnw.net/emoticons/v1/25/3.0",
                  },
                  start: 46,
                  end: 50,
                },
              ],
              msgId: "43285909-412c-4eee-b80d-89f72ba53142",
            },
            renderedText:
              'Howdy! My name is Bill and I am here to serve <img src="https://static-cdn.jtvnw.net/emoticons/v1/25/1.0" srcset="https://static-cdn.jtvnw.net/emoticons/v1/25/1.0 1x, https://static-cdn.jtvnw.net/emoticons/v1/25/1.0 2x, https://static-cdn.jtvnw.net/emoticons/v1/25/3.0 4x" title="Kappa" class="emote">',
          },
        },
      });
      window.dispatchEvent(emulated);
    }
    return;
  }
  if (obj.detail.listener === "delete-message") {
    const msgId = obj.detail.event.msgId;
    $(`.message-row[data-msgid=${msgId}]`).remove();
    return;
  } else if (obj.detail.listener === "delete-messages") {
    const sender = obj.detail.event.userId;
    $(`.message-row[data-sender=${sender}]`).remove();
    return;
  }

if (obj.detail.listener !== "message") return;
let data = obj.detail.event.data;
if (data.text.startsWith("!") && hideCommands === "yes") return;
if (ignoredUsers.indexOf(data.nick) !== -1) return;
let message = attachEmotes(data);
let badges = "",
badge;
if (provider === "mixer") {
data.badges.push({ url: data.avatar });
}
for (let i = 0; i < data.badges.length; i++) {
badge = data.badges[i];
badges += `<img alt="" src="${badge.url}" class="badge ${badge.type}-icon"> `;
}
let username = data.displayName + ":";
if (nickColor === "user") {
const color =
data.displayColor !== ""
? data.displayColor
: "#" + md5(username).slice(26);
username = `<span style="color:${color}">${username}</span>`;
} else if (nickColor === "custom") {
const color = customNickColor;
username = `<span style="color:${color}">${username}</span>`;
} else if (nickColor === "remove") {
username = "";
}
addMessage(username, badges, message, data.isAction, data.userId, data.msgId);
});

window.addEventListener("onWidgetLoad", function (obj) {
const fieldData = obj.detail.fieldData;
animationIn = fieldData.animationIn;
animationOut = fieldData.animationOut;
hideAfter = fieldData.hideAfter;
messagesLimit = fieldData.messagesLimit;
nickColor = fieldData.nickColor;
customNickColor = fieldData.customNickColor;
hideCommands = fieldData.hideCommands;
channelName = obj.detail.channel.username;
fetch(
"https://api.streamelements.com/kappa/v2/channels/" +
obj.detail.channel.id +
"/"
)
.then((response) => response.json())
.then((profile) => {
provider = profile.provider;
});
if (fieldData.alignMessages === "block") {
addition = "prepend";
removeSelector = ".message-row:nth-child(n+" + (messagesLimit + 1) + ")";
} else {
addition = "append";
removeSelector =
".message-row:nth-last-child(n+" + (messagesLimit + 1) + ")";
}

ignoredUsers = fieldData.ignoredUsers
.toLowerCase()
.replace(" ", "")
.split(",");
});

function attachEmotes(message) {
let text = html_encode(message.text);
let data = message.emotes;
if (typeof message.attachment !== "undefined") {
if (typeof message.attachment.media !== "undefined") {
if (typeof message.attachment.media.image !== "undefined") {
text = `${message.text}<img src="${message.attachment.media.image.src}">`;
}
}
}
return text.replace(/([^\s]\*)/gi, function (m, key) {
let result = data.filter((emote) => {
return html_encode(emote.name) === key;
});
if (typeof result[0] !== "undefined") {
let url = result[0]["urls"][1];
if (provider === "twitch") {
return `<img class="emote" " src="${url}"/>`;
} else {
if (typeof result[0].coords === "undefined") {
result[0].coords = { x: 0, y: 0 };
}
let x = parseInt(result[0].coords.x);
let y = parseInt(result[0].coords.y);

        let width = "{emoteSize}px";
        let height = "auto";

        if (provider === "mixer") {
          console.log(result[0]);
          if (result[0].coords.width) {
            width = `${result[0].coords.width}px`;
          }
          if (result[0].coords.height) {
            height = `${result[0].coords.height}px`;
          }
        }
        return `<div class="emote" style="width: ${width}; height:${height}; display: inline-block; background-image: url(${url}); background-position: -${x}px -${y}px;"></div>`;
      }
    } else return key;

});
}

function html_encode(e) {
return e.replace(/[<>"^]/g, function (e) {
return "&#" + e.charCodeAt(0) + ";";
});
}

function addMessage(username, badges, message, isAction, uid, msgId) {
totalMessages += 1;
let actionClass = "";
if (isAction) {
actionClass = "action";
}
const element = $.parseHTML(`
    <div data-sender="${uid}" data-msgid="${msgId}" class="message-row {animationIn} animated" id="msg-${totalMessages}">
<div class="user-box ${actionClass}">${badges}${username}</div>
<div class="user-message ${actionClass}">${message}</div>
</div>`);
if (addition === "append") {
if (hideAfter !== 999) {
$(element)
.appendTo(".main-container")
.delay(hideAfter _ 1000)
.queue(function () {
$(this)
.removeClass(animationIn)
.addClass(animationOut)
.delay(1000)
.queue(function () {
$(this).remove();
})
.dequeue();
});
} else {
$(element).appendTo(".main-container");
}
} else {
if (hideAfter !== 999) {
$(element)
.prependTo(".main-container")
.delay(hideAfter _ 1000)
.queue(function () {
$(this)
.removeClass(animationIn)
.addClass(animationOut)
.delay(1000)
.queue(function () {
$(this).remove();
})
.dequeue();
});
} else {
$(element).prependTo(".main-container");
}
}

if (totalMessages > messagesLimit) {
removeRow();
}
}

function removeRow() {
if (!$(removeSelector).length) {
    return;
  }
  if (animationOut !== "none" || !$(removeSelector).hasClass(animationOut)) {
if (hideAfter !== 999) {
$(removeSelector).dequeue();
} else {
$(removeSelector)
.addClass(animationOut)
.delay(1000)
.queue(function () {
$(this).remove().dequeue();
});
}
return;
}

$(removeSelector).animate(
{
height: 0,
opacity: 0,
},
"slow",
function () {
$(removeSelector).remove();
}
);
}
```

### Add the base FIELDS

Copy the code from [StreamElements' custom chat widget.json](https://github.com/StreamElements/widgets/blob/master/CustomChat/widget.json) and paste it in the FIELDS section.

It should look like this:

```json
{
  "testMessage": {
    "type": "button",
    "label": "Test message",
    "value": "Test message",
    "group": "Test"
  },
  "fontName": {
    "type": "text",
    "label": "Font name",
    "value": "Montserrat",
    "group": "Typography"
  },
  "fontSize": {
    "type": "number",
    "label": "Font size",
    "value": 24,
    "group": "Typography"
  },
  "fontWeight": {
    "label": "Font Weight",
    "type": "dropdown",
    "value": "400",
    "options": {
      "100": "Thin (100)",
      "300": "Light (300)",
      "400": "Regular (400)",
      "500": "Medium (500)",
      "700": "Bold (700)",
      "900": "Black (900)"
    },
    "group": "Typography"
  },
  "fontColor": {
    "type": "colorpicker",
    "label": "Font Color",
    "value": "rgba(255,255,255,1)",
    "group": "Typography"
  },
  "textShadow": {
    "type": "text",
    "label": "Text shadow",
    "value": "rgb(0, 0, 0) 1px 1px 1px",
    "group": "Typography"
  },
  "emoteSize": {
    "type": "number",
    "label": "Emote size",
    "value": 24,
    "group": "Typography"
  },
  "alignMessages": {
    "label": "Align Messages",
    "type": "dropdown",
    "value": "flex",
    "options": {
      "flex": "Bottom",
      "block": "Top"
    },
    "group": "Typography"
  },
  "nickColor": {
    "type": "dropdown",
    "label": "Nickname color:",
    "value": "user",
    "options": {
      "user": "as on twitch",
      "messagecolor": "same as message text",
      "custom": "specified below",
      "remove": "Remove username"
    },
    "group": "Typography"
  },
  "customNickColor": {
    "type": "colorpicker",
    "label": "Custom nickname color:",
    "value": "rgba(0,255,0,1)",
    "group": "Typography"
  },
  "displayBadges": {
    "type": "dropdown",
    "label": "Display Badges",
    "value": "inline",
    "options": {
      "inline": "Yes",
      "none": "No"
    },
    "group": "Settings"
  },
  "messagesLimit": {
    "type": "number",
    "label": "Messages limit",
    "value": 4,
    "group": "Settings"
  },
  "hideAfter": {
    "type": "number",
    "label": "Hide after seconds (999 to disable)",
    "value": 999,
    "group": "Settings"
  },
  "hideCommands": {
    "label": "Hide lines starting with ! (!command)",
    "type": "dropdown",
    "value": "yes",
    "options": {
      "yes": "Yes",
      "no": "No"
    },
    "group": "Settings"
  },
  "ignoredUsers": {
    "label": "Ignored users (comma separated)",
    "type": "text",
    "value": "StreamElements,OtherBot",
    "group": "Settings"
  },
  "backgroundColor": {
    "type": "colorpicker",
    "label": "Background color:",
    "value": "rgba(0,0,0,0)",
    "group": "Settings"
  },
  "animationIn": {
    "type": "dropdown",
    "label": "Animation In:",
    "value": "none",
    "options": {
      "none": "None",
      "bounceIn": "bounceIn",
      "bounceInDown": "bounceInDown",
      "bounceInLeft": "bounceInLeft",
      "bounceInRight": "bounceInRight",
      "bounceInUp": "bounceInUp",
      "fadeIn": "fadeIn",
      "fadeInDown": "fadeInDown",
      "fadeInDownBig": "fadeInDownBig",
      "fadeInLeft": "fadeInLeft",
      "fadeInLeftBig": "fadeInLeftBig",
      "fadeInRight": "fadeInRight",
      "fadeInRightBig": "fadeInRightBig",
      "fadeInUp": "fadeInUp",
      "fadeInUpBig": "fadeInUpBig",
      "flipInX": "flipInX",
      "flipInY": "flipInY",
      "lightSpeedIn": "lightSpeedIn",
      "rotateIn": "rotateIn",
      "rotateInDownLeft": "rotateInDownLeft",
      "rotateInDownRight": "rotateInDownRight",
      "rotateInUpLeft": "rotateInUpLeft",
      "rotateInUpRight": "rotateInUpRight",
      "slideInUp": "slideInUp",
      "slideInDown": "slideInDown",
      "slideInLeft": "slideInLeft",
      "slideInRight": "slideInRight",
      "zoomIn": "zoomIn",
      "zoomInDown": "zoomInDown",
      "zoomInLeft": "zoomInLeft",
      "zoomInRight": "zoomInRight",
      "zoomInUp": "zoomInUp",
      "jackInTheBox": "jackInTheBox",
      "rollIn": "rollIn"
    },
    "group": "Settings"
  },
  "animationOut": {
    "type": "dropdown",
    "label": "Animation Out:",
    "value": "none",
    "options": {
      "none": "None",
      "bounceOut": "bounceOut",
      "bounceOutDown": "bounceOutDown",
      "bounceOutLeft": "bounceOutLeft",
      "bounceOutRight": "bounceOutRight",
      "bounceOutUp": "bounceOutUp",
      "fadeOut": "fadeOut",
      "fadeOutDown": "fadeOutDown",
      "fadeOutDownBig": "fadeOutDownBig",
      "fadeOutLeft": "fadeOutLeft",
      "fadeOutLeftBig": "fadeOutLeftBig",
      "fadeOutRight": "fadeOutRight",
      "fadeOutRightBig": "fadeOutRightBig",
      "fadeOutUp": "fadeOutUp",
      "fadeOutUpBig": "fadeOutUpBig",
      "flipOutX": "flipOutX",
      "flipOutY": "flipOutY",
      "lightSpeedOut": "lightSpeedOut",
      "rotateOut": "rotateOut",
      "rotateOutDownLeft": "rotateOutDownLeft",
      "rotateOutDownRight": "rotateOutDownRight",
      "rotateOutUpLeft": "rotateOutUpLeft",
      "rotateOutUpRight": "rotateOutUpRight",
      "slideOutUp": "slideOutUp",
      "slideOutDown": "slideOutDown",
      "slideOutLeft": "slideOutLeft",
      "slideOutRight": "slideOutRight",
      "zoomOut": "zoomOut",
      "zoomOutDown": "zoomOutDown",
      "zoomOutLeft": "zoomOutLeft",
      "zoomOutRight": "zoomOutRight",
      "zoomOutUp": "zoomOutUp",
      "rollOut": "rollOut"
    },
    "group": "Settings"
  },
  "widgetName": {
    "type": "hidden",
    "value": "Custom chat"
  },
  "widgetAuthor": {
    "type": "hidden",
    "value": "lx"
  },
  "widgetVersion": {
    "type": "hidden",
    "value": "1.0"
  },
  "widgetUpdateUrl": {
    "type": "hidden",
    "value": "https://github.com/StreamElements/widgets/blob/master/CustomChat/"
  }
}
```

You have now added the base code for your widget in StreamElements. You can start experimenting with the overlay settings from here, or keep following along to add more customization.

<br>

## Customizing Your Widget In StreamElements

To really give the chat widget your own flair, let's start customizing.

We'll be going for a cute minimal look, like this:

![](/img/posts/custom-chat-widget-tutorial.png)

As you can see, I've added some new username and message box styling. In order to achieve this, we need to change the CSS.

I've also added some new fields in the left sidebar.

Let's start there first.

### Add the custom FIELDS

For the Test Message, I decided to change the "Test" field to a "Preview" field.

Here's the code for that:

```json
  "testMessage": {
    "type": "button",
    "label": "Send test message",
    "value": "Test message",
    "group": "Preview"
  },
```

Then for the overall styling, I decided to add a "Global Styles" field.

It looks like this:

```json
  "fontName": {
    "type": "text",
    "label": "Font name",
    "value": "Montserrat",
    "group": "Global Styles"
  },
  "fontSize": {
    "type": "number",
    "label": "Font size",
    "value": 24,
    "group": "Global Styles"
  },
  "fontWeight": {
    "label": "Font weight",
    "type": "dropdown",
    "value": "400",
    "options": {
      "100": "Thin (100)",
      "300": "Light (300)",
      "400": "Regular (400)",
      "500": "Medium (500)",
      "700": "Bold (700)",
      "900": "Black (900)"
    },
    "group": "Global Styles"
  },
  "fontColor": {
    "type": "colorpicker",
    "label": "Font color",
    "value": "rgba(255,255,255,1)",
    "group": "Global Styles"
  },
  "messagebackgroundColor": {
    "type": "colorpicker",
    "label": "Background color",
    "value": "rgba(0,0,0,0)",
    "group": "Global Styles"
  },
  "alignMessages": {
    "label": "Align messages",
    "type": "dropdown",
    "value": "flex",
    "options": {
      "flex": "Bottom",
      "block": "Top"
    },
    "group": "Global Styles"
  },
  "animationIn": {
    "type": "dropdown",
    "label": "Animation in",
    "value": "none",
    "options": {
      "none": "None",
      "bounceIn": "bounceIn",
      "bounceInDown": "bounceInDown",
      "bounceInLeft": "bounceInLeft",
      "bounceInRight": "bounceInRight",
      "bounceInUp": "bounceInUp",
      "fadeIn": "fadeIn",
      "fadeInDown": "fadeInDown",
      "fadeInDownBig": "fadeInDownBig",
      "fadeInLeft": "fadeInLeft",
      "fadeInLeftBig": "fadeInLeftBig",
      "fadeInRight": "fadeInRight",
      "fadeInRightBig": "fadeInRightBig",
      "fadeInUp": "fadeInUp",
      "fadeInUpBig": "fadeInUpBig",
      "flipInX": "flipInX",
      "flipInY": "flipInY",
      "lightSpeedIn": "lightSpeedIn",
      "rotateIn": "rotateIn",
      "rotateInDownLeft": "rotateInDownLeft",
      "rotateInDownRight": "rotateInDownRight",
      "rotateInUpLeft": "rotateInUpLeft",
      "rotateInUpRight": "rotateInUpRight",
      "slideInUp": "slideInUp",
      "slideInDown": "slideInDown",
      "slideInLeft": "slideInLeft",
      "slideInRight": "slideInRight",
      "zoomIn": "zoomIn",
      "zoomInDown": "zoomInDown",
      "zoomInLeft": "zoomInLeft",
      "zoomInRight": "zoomInRight",
      "zoomInUp": "zoomInUp",
      "jackInTheBox": "jackInTheBox",
      "rollIn": "rollIn"
    },
    "group": "Global Styles"
  },
  "animationOut": {
    "type": "dropdown",
    "label": "Animation out",
    "value": "none",
    "options": {
      "none": "None",
      "bounceOut": "bounceOut",
      "bounceOutDown": "bounceOutDown",
      "bounceOutLeft": "bounceOutLeft",
      "bounceOutRight": "bounceOutRight",
      "bounceOutUp": "bounceOutUp",
      "fadeOut": "fadeOut",
      "fadeOutDown": "fadeOutDown",
      "fadeOutDownBig": "fadeOutDownBig",
      "fadeOutLeft": "fadeOutLeft",
      "fadeOutLeftBig": "fadeOutLeftBig",
      "fadeOutRight": "fadeOutRight",
      "fadeOutRightBig": "fadeOutRightBig",
      "fadeOutUp": "fadeOutUp",
      "fadeOutUpBig": "fadeOutUpBig",
      "flipOutX": "flipOutX",
      "flipOutY": "flipOutY",
      "lightSpeedOut": "lightSpeedOut",
      "rotateOut": "rotateOut",
      "rotateOutDownLeft": "rotateOutDownLeft",
      "rotateOutDownRight": "rotateOutDownRight",
      "rotateOutUpLeft": "rotateOutUpLeft",
      "rotateOutUpRight": "rotateOutUpRight",
      "slideOutUp": "slideOutUp",
      "slideOutDown": "slideOutDown",
      "slideOutLeft": "slideOutLeft",
      "slideOutRight": "slideOutRight",
      "zoomOut": "zoomOut",
      "zoomOutDown": "zoomOutDown",
      "zoomOutLeft": "zoomOutLeft",
      "zoomOutRight": "zoomOutRight",
      "zoomOutUp": "zoomOutUp",
      "rollOut": "rollOut"
    },
    "group": "Global Styles"
  },
  "emoteSize": {
    "type": "number",
    "label": "Emote size",
    "value": 24,
    "group": "Global Styles"
  },
  "sliderMessage": {
    "label": "Horizontal message position",
    "type": "slider",
    "value": "1",
    "max": 10,
    "min": 0,
    "step": 1,
    "value": 0,
    "group": "Global Styles"
  },
```

To customize the username box, let's add "Username Styles".

It looks like this:

```json
  "nickColor": {
    "type": "dropdown",
    "label": "Username color",
    "value": "user",
    "options": {
      "user": "As on twitch",
      "messagecolor": "Same as message text",
      "custom": "Specified below",
      "remove": "Remove username"
    },
    "group": "Username Styles"
  },
  "customNickColor": {
    "type": "colorpicker",
    "label": "Custom username color",
    "value": "rgba(0,255,0,1)",
    "group": "Username Styles"
  },
  "userbackgroundColor": {
    "type": "colorpicker",
    "label": "Username background color",
    "value": "rgba(0,0,0,1)",
    "group": "Username Styles"
  },
  "displayBadges": {
    "type": "dropdown",
    "label": "Display badges",
    "value": "inline",
    "options": {
      "inline": "Yes",
      "none": "No"
    },
    "group": "Username Styles"
  },
  "sliderNickname": {
    "label": "Horizontal username position",
    "type": "slider",
    "value": "1",
    "max": 25,
    "min": 0,
    "step": 1,
    "value": 0,
    "group": "Username Styles"
  },
```

For the message box, let's add "Message Settings".

It looks like this:

```json
  "messagesLimit": {
    "type": "number",
    "label": "Message limit",
    "value": 4,
    "group": "Message Settings"
  },
  "hideAfter": {
    "type": "number",
    "label": "Hide after seconds (999 to disable)",
    "value": 999,
    "group": "Message Settings"
  },
  "hideCommands": {
    "label": "Hide lines starting with ! (!command)",
    "type": "dropdown",
    "value": "yes",
    "options": {
      "yes": "Yes",
      "no": "No"
    },
    "group": "Message Settings"
  },
  "ignoredUsers": {
    "label": "Ignored users (comma separated)",
    "type": "text",
    "value": "StreamElements,OtherBot",
    "group": "Message Settings"
  },
```

Lastly, let's add an "About" section that includes information about the widget.

It looks like this:

```json
  "widgetName": {
    "label": "Custom Chat",
    "type": "hidden",
    "value": "Custom Chat",
    "group": "About"
  },
  "widgetAuthor": {
    "label": "Tutorial by glitchedinorbit",
    "type": "hidden",
    "value": "glitchedinorbit",
    "group": "About"
  },
  "widgetVersion": {
    "type": "hidden",
    "value": "1.0",
    "group": "About"
  },
  "widgetUpdateUrl": {
    "type": "hidden",
    "value": "https://",
    "group": "About"
  }
```

### Add the custom CSS

Next, we'll tackle the CSS portion of the chat widget.

So let's add properties in between `@import` and `* {` that allow full control over the way our widget looks.

```css
html,
body {
  margin: 0;
  padding: 0;
}

html {
  overflow: hidden;
  font-size: var(--baseFontSize);
}
```

From the `.main-container` section, let's delete everything except for the following:

```css
.main-container{
  display:{{alignMessages}};
  flex-direction: column;
  justify-content: flex-end;
  height:98%;
}
```

The `user-box` section applies to everything regarding the box where the username is. From this section, let's change and delete some properties:

```css
/* Username box */
.user-box{
  position: absolute;
  background-color: {{userbackgroundColor}};
  font-size:{{fontSize}}px;
  font-weight: 800;
  z-index: 99;
  top: 2%;
  padding: 18px;
  margin-top: auto;
  left: {{sliderNickname}}%;
  text-align: left;
  width: block;
  margin-left: 20px;
  margin-right: 20px;
  border-radius: 10px;
  text-transform: uppercase;
}

.user-box > span{
  font-size:{{fontSize}}px;
  text-align: left;
  margin-right: 20px;
}
```

Both the `.message-row` and `.user-message` apply to the box and actual message underneath the username. From this section, let's change and delete some properties as well:

```css
/* Message box */
.message-row{
  position: relative;
  flex: 0 0 auto;
  margin-bottom:20px;
  vertical-align: baseline;
  padding-top: 40px;
  padding-right: 18px;
  text-align: left;
  margin-right: 20px;
}

.user-message{
  display:inline-block;
  font-size:{{fontSize}}px;
  background-color: {{messagebackgroundColor}};
  height: auto;
  top: 5.5%;
  max-width:95%;
  position: relative;
  word-wrap: break-word;
  padding: 18px;
  left: {{sliderMessage}}%;
  margin-left: 20px;
  margin-right: 20px;
  border-radius: 10px;
}
```

Lastly, let's set properties for the custom stuff regarding `.badge`, `.emote`, and `.action`, like so:

```css
/* Custom stuff */
.badge{
  display: {{displayBadges}};
  height: 1em;
  position: relative;
  line-height: 1em;
  vertical-align: middle;
  top: -0.1em;
}

.emote{
  height: {emoteSize}px;
  vertical-align: middle;
  background-repeat:no-repeat;
}

.action{
  font-style: italic;
}
```

We'll leave the HTML and JS sections as they are.

### Add the custom DATA

We still need to add the DATA, so let's do that. This code will display in one single line:

```json
{
  "eventsLimit": 7,
  "includeFollowers": "yes",
  "includeRedemptions": "yes",
  "includeHosts": "yes",
  "minHost": 1,
  "includeRaids": "yes",
  "minRaid": 1,
  "includeSubs": "yes",
  "includeTips": "yes",
  "minTip": 1,
  "includeCheers": "yes",
  "minCheer": 1,
  "direction": "top",
  "textOrder": "nameFirst",
  "fadeoutTime": 999,
  "fontColor": "#ffffff",
  "theme": "texture",
  "backgroundOpacity": 50,
  "backgroundColor": "#a3aaa9",
  "iconColor": "rgb(255, 255, 255, 255)",
  "locale": "en-US",
  "testMessage": "Test message",
  "fontName": "Poppins",
  "fontSize": 14,
  "fontWeight": "500",
  "textShadow": "",
  "emoteSize": 20,
  "alignMessages": "flex",
  "nickColor": "messagecolor",
  "customNickColor": "#feffff",
  "displayBadges": "none",
  "messagesLimit": 12,
  "hideAfter": 999,
  "hideCommands": "yes",
  "ignoredUsers": "StreamElements,OtherBot",
  "animationIn": "fadeInLeftBig",
  "animationOut": "none",
  "widgetName": "Custom Chat",
  "widgetAuthor": "glitchedinorbit",
  "widgetVersion": "1.0",
  "widgetUpdateUrl": "https://github.com/{your-github-repository}",
  "userbackgroundColor": "#99b5ff",
  "messagebackgroundColor": "#242a38",
  "sliderNickname": 0,
  "sliderMessage": 10
}
```

You've now completed the custom chat widget!

Customize it as you see fit from the overlay settings in StreamElements, or with your own CSS in the editor.

<br>

## Backing Up Your Code

In order to really make sure we can mess with the code in StreamElements without losing our working widget prototype, let's make a backup of our code.

### Organize your code

Install and open up [Visual Studio Code](https://code.visualstudio.com).

Create a new folder called "CustomChatWidget" or however you have named your chat widget.

In this folder, create a file named `widget.css`. Copy the code from CSS into this file.

Create a file named `widget.html`. Copy the code from HTML into this file.

Create a file named `widget.js`. Copy the code from JS into this file.

Create a file named `widget.json`. Copy the code from FIELDS into this file.

### Upload your code to GitHub

Sign up for or log in to your existing [GitHub account](https://github.com).

In GitHub, create a new private repository called "streamelements-widgets" or something similar that allows users to easily find your custom StreamElements widgets in case you'd like to share the code.

> <i class="bi bi-pencil-fill"></i>&nbsp;&nbsp;Note: You can always make your repository public later in case you wish to share your code.

Back in Visual Studio Code, open up Source Control (Ctrl+Shift+G) to connect your GitHub repository.

When connected, simply put the `widget.css`, `widget.html`, `widget.js`, and `widget.json` files in your current repository. Click the "Commit" button in Source Control commit the code to GitHub.

Back in StreamElements, under the Fields tab, scroll all the way to the bottom to include your own widget update URL.

It should look like this:

```json
  },
  "widgetUpdateUrl": {
    "type": "hidden",
    "value": "https://github.com/{your-github-repository}"
  }
```

Commit your updated code to GitHub as necessary.

<br>

## Sharing And Selling Your StreamElements Widgets

Perhaps you'd love to share your widgets with your community or via an online shop.

Unfortunately, there currently isn't really an easy way to share your StreamElements overlays.

But we do have some workarounds.

### Share some text files

You can simply copy the code from the overlay and paste it in separate text files. This is by far the most feasible way to share the widget code for most people.

Create a new folder with your chat widget's name.

Inside this folder, create five `.txt` files. Name them as follows: `css.txt`, `data.txt`, `fields.txt`, `html.txt`, and `js.txt`.

Go to the existing StreamElements widget overlay page.

Copy the code from each section in the Editor and paste it in its designated `.txt` file.

Include instructions in a `README.txt` file if needed.

Compress (zip) the entire folder with [7ZIP](https://7-zip.org).

Then simply upload the compressed folder to your preferred social messaging app or online shop.

### Share your widget in the StreamElements Discord

You can also try sharing your widget on the StreamElements Discord server, after applying in `#widget-share`.

> <i class="bi bi-pencil-fill"></i>&nbsp;&nbsp;Note: According to one of the Discord members, there's a backlog of applications that hasn't been approved yet. So it's uncertain how long approval will take and if this method even works.

After approval, you should be able to share your overlay link.

To do so, copy the full overlay link into a text editor.

Rename the link to include `/dashboard/overlays/share` instead of `/overlay`, and to exclude the `/editor?er=1` part.

So starting from `https://streamelements.com/overlay/<code>/editor?er=1`, it should now look like `https://streamelements.com/dashboard/overlays/share/<code>`.

Share this final link in your community and online shops.

### Apply for StreamElements' Overlay Sharing Application

If you're eligible, you can apply for [Streamelements' One-Click Overlay Sharing Application](https://docs.google.com/forms/d/e/1FAIpQLSdece7hRCA9F3TRh5dLMIygUd5PlWa4xfb1wraW46yvyqs2Ww/viewform).

> <i class="bi bi-pencil-fill"></i>&nbsp;&nbsp;Note: This application only applies to businesses and eligible participants of exclusive SE Community programs.

After approval, you should be able to share your overlay link.

## To Wrap Up

Congrats on making it all the way through.

I hope you found this step by step tutorial helpful.

I'm pretty sure the code can look better. And I'd love to integrate events for follows, subs, bits, etc., but I don't know how to do that yet. So please let me know if you have any tips!

Thanks for reading.

<br>

<details>
    <summary>Sources</summary>

    <br>

    <span style="font-size:14px;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;C4ldas [@c4ldas]. (2023, October 31). <i>I don't want to disappoint anyone, but I should say that to avoid frustrations... I'm not sure when submitted widgets are going to be released again, it seems they are not working on that anymore.</i> [Reply] StreamElements Discord.</span>

    <br>
    <br>

    <span style="font-size:14px;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;C4ldas [@c4ldas]. (2023, October 31). <i>Sharing link is for companies who sell widgets, SE partners or people who shared widgets with the community.</i> [Reply] StreamElements Discord.</span>

    <br>
    <br>

    <span style="font-size:14px;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;StreamElements. (n.d.). <i>Custom Widgets.</i> StreamElements Developer Portal. https://dev.streamelements.com/docs/api-docs/775038fd4f4a9-stream-elements-custom-widgets</span>

</details>

<br>
