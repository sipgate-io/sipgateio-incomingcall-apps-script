# sipgateio-incomingcall-apps-script

<img src="https://www.sipgatedesign.com/wp-content/uploads/wort-bildmarke_positiv_2x.jpg" alt="sipgate logo" title="sipgate" align="right" height="112" width="200"/>

This example demonstrates how to receive and process webhooks from [sipgate.io](https://developer.sipgate.io/) with Google Apps Script.

- [Prerequisites](#Prerequisites)
- [Enabling sipgate.io for your sipgate account](#Enabling-sipgateio-for-your-sipgate-account)
- [How sipgate.io webhooks work](#How-sipgateio-webhooks-work)
- [How to setup an apps script](#How-to-setup-an-apps-script)
- [Configure webhooks for sipgate.io](#Configure-webhooks-for-sipgateio)
- [Execution](#Execution)
- [Common Issues](#Common-Issues)
- [Related](#Related)
- [Contact Us](#Contact-Us)
- [License](#License)
- [External Libraries](#External-Libraries)


## Prerequisites
- sipgate.io-Account
- Google-Account


## Enabling sipgate.io for your sipgate account
In order to use sipgate.io, you need to book the corresponding package in your sipgate account. The most basic package is the free **sipgate.io S** package.

## How sipgate.io webhooks work

### What is a webhook?
A webhook is a POST request that sipgate.io makes to a predefined URL when a certain event occurs.
These requests contain information about the event that occurred in `application/x-www-form-urlencoded` format.

This is an example payload converted from `application/x-www-form-urlencoded` to json:
```json
{
  "event": "newCall",
  "direction": "in",
  "from": "492111234567",
  "to": "4915791234567",
  "callId":"12345678",
  "origCallId":"12345678",
  "user": [ "Alice" ],
  "xcid": "123abc456def789",
  "diversion": "1a2b3d4e5f"
}
```

### sipgate.io webhook events
sipgate.io offers webhooks for the following events:

- **newCall:** is triggered when a new incoming or outgoing call occurs 
- **onAnswer:** is triggered when a call is answered – either by a person or an automatic voicemail
- **onHangup:** is triggered when a call is hung up
- **dtmf:** is triggered when a user makes an entry of digits during a call

**Note:** Per default sipgate.io only sends webhooks for **newCall** events.
Usually to subscribe to other event types you can reply to the **newCall** event with an XML response. 
Unfortunately Google Apps Script does not support `content-type: application/xml`.Therefore we cannot subscribe to additional event types.
If you need to respond you may refer to our repository [sipgateio-incomingcall-node](https://github.com/sipgate-io/sipgateio-incomingcall-node).

## How to setup an apps script
First navigate to [Google Apps Script](https://script.google.com/home) and create a new project via the button in the upper left corner.
You are redirected to a new unnamed project.
![untitled_project](./incomingCall_untitledProject.png)
Replace the code in the `Code.gs`with the following: 
```
function doPost(e) {
    var url = "YOUR_GOOGLE_SPREADSHEET_URL";
    var sheet = SpreadsheetApp.openByUrl(url);
    
    var data = JSON.parse(JSON.stringify(e));
    
    sheet.appendRow(["event: ", "from: ", "to: "]);
    sheet.appendRow([data.parameters.event.toString(), data.parameters.from.toString(), data.parameters.to.toString()]);
}
```

Du erstellst in deinem Google Drive ein neues Spreadsheet und öffnest dieses. Dann kopierst du die URL des Sheets und fügst sie in das Feld `"YOUR_GOOGLE_SPREADSHEET_URL"` ein.

Dann erstellen  wir ein neues Deployment, indem wir auf den blauen Button `Deploy` in der oberen rechten Ecke klicken. 
new deployment
select type: web app
fill out: execute as me ; who has access anyone
deploy
authorize; allow
copy web app url; done
sipgate incoming url
anrufen 
fertig

### What happens here?
The `doPost`-function will receive the webhook event as an `Object`. We then extract data from this object and store it in a Google Spreadsheet.

## Configure webhooks for sipgate.io 
You can configure webhooks for sipgate.io as follows:

1. Navigate to [console.sipgate.com](https://console.sipgate.com/) and login with your sipgate account credentials.
2. Select the **Webhooks**&nbsp;>&nbsp;**URLs** tab in the left side menu
3. Click the gear icon of the **Incoming** or **Outgoing** entry
4. Fill in your webhook URL and click save. The webhook URL is the web app URL you will get by deploying the apps script.

## Execution

## Common Issues

### web app displays "Feature sipgate.io not booked."
Possible reasons are:
- the sipgate.io feature is not booked for your account

See the section [Enabling sipgate.io for your sipgate account](#enabling-sipgateio-for-your-sipgate-account) for instruction on how to book sipgate.io


### "Error: listen EADDRINUSE: address already in use :::{port}"
Possible reasons are:
- another instance of the application is already running
- the specified port is in use by another application


### "Error: listen EACCES: permission denied 0.0.0.0:{port}"
Possible reasons are:
- you do not have permission to bind to the specified port.
  This usually occurs if you try to use port 80, 443 or another well-known port which can only be bound with superuser privileges


### Call happened but no webhook was received 
Possible reasons are:
- the configured webhook URL is incorrect
- the SSH tunnel connection broke
- webhooks are not enabled for the phoneline that received the call


## Related
- [Express](https://expressjs.com/)


## Contact Us
Please let us know how we can improve this example.
If you have a specific feature request or found a bug, please use **Issues** or fork this repository and send a **pull request** with your improvements.


## License
This project is licensed under **The Unlicense** (see [LICENSE file](./LICENSE)).


## External Libraries
This code uses the following external libraries

- _Express_:
  - Licensed under the [MIT License](https://opensource.org/licenses/MIT)
  - Website: https://expressjs.com/


---

[sipgate.io](https://www.sipgate.io) | [@sipgateio](https://twitter.com/sipgateio) | [API-doc](https://api.sipgate.com/v2/doc)
