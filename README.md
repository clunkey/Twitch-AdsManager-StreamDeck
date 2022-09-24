# Twitch-AdsManager-StreamDeck
**Snooze** or **Start** Ads Manager ads with a **Stream Deck**

API Ninja templates based on TheShiningOne's excellent [StreamerBot guide](https://www.youtube.com/watch?v=ORb1EGwZRPA). 

**All credit to TheShiningOne for discovering this method**! Original repo [here](https://github.com/TheShining1/Twitch-API).

## Important
1. **Do not stream or share your request data**. Treat your OAuth string like a password.
2. There is no public documentation for these requests, and Twitch may break them without warning.
3. You will have to go live for a short time to gather the necessary info. A savvy enough streamer could complete the [Getting Started](#getting-started) steps during an actual ad break. Others can just briefly annoy their viewers with a classic "testing, please ignore" stream (*NOTE: in my experience, they don't ignore*).

## Getting Started

First, we need to copy the POST request that's made whenever we click **Snooze** or **Run Ad** the Ads Manager. This is covered in the first 2 minutes of the [video](https://www.youtube.com/watch?v=ORb1EGwZRPA) above. **I recommend watching it if you aren't familiar with POST requests or browser dev tools**.

1. Go to your [Creator Dashboard](https://dashboard.twitch.tv/u/maclunkeyculkin/stream-manager)
2. Make sure the [Ads Manager](https://help.twitch.tv/s/article/ads-manager) is on your Dashboard, and your **Ads Schedule** is enabled
3. Open your browser's dev tools (depending on the browser: `Ctrl + Shift + J`, `Ctrl + Shift + I`, or `F12`)
4. Open the **Network** tab

   - OPTIONAL: before going live, you may want to get comfortable reading the **Network** feed. In your **Creator Dashboard**, click the **Edit Stream Info** Quick Action button, and note the new rows that populate in the **Network** tab with the name `gql`. Click one of them and navigate to its **Payload** tab (or **Request** in some browsers). Note the request's `operationName`, and try to find the one whose `operationName` is `EditBroadcastContextQuery`. At any time, you can pause or resume the Network feed (`Ctrl + E` in Chrome) so your requests don't get buried. Once you feel comfortable locating requests, move on to the next step.

5. Go live on Twitch and pause the stream if it's playing in your Creator Dashboard
6. Once the **Snooze** and **Run Ad** buttons are clickable in the Ads Manager, follow these steps for each button:
   * Click the button. At least one row should pop up in the **Network** tab with the Name `gql`
   * Click each `gql` request and check its **Payload** tab (or **Request** in some browsers) until you find the one whose `operationName` equals either `SnoozeAd` or `AutoAdsFrequencySelector_StartAd`, based on the button you clicked.
   * Right-click that request, click **Copy as cURL (cmd)**, and paste the results into a Notepad

We now have everything we need to recreate those requests. Open up the Stream Deck app and download the [API Ninja plug-in](https://apps.elgato.com/plugins/com.barraider.apininja)

## [SnoozeAd.ninja](SnoozeAd.ninja)
In the Stream Deck app, create a new API Ninja action. Scroll to the bottom of the action's settings and click **Import Settings** to import [SnoozeAd.ninja](SnoozeAd.ninja), then replace the bracketed placeholders with the values from your SnoozeAd `gql` request:

in **Headers**:

* `Authorization: {YOUR_OAUTH}` <-- e.g. `OAuth abcdefghijklmnopqrstuvwxyz1234`

in **Data**:
* `"targetChannelID": "{YOUR_CHANNEL_ID}"` <-- e.g. `"1234567"`
* `"sha256Hash": "{YOUR_SHA_HASH}"` <-- e.g. `"abcdefghijklmnopqrstuvwxyz123456abcdefghijklmnopqrstuvwxyz123456"`, there will be a different hash for each button; don't copy from one to the other

## [StartAd.ninja](StartAd.ninja)
In the Stream Deck app, create a new API Ninja action. Scroll to the bottom of the action's settings and click **Import Settings** to import [StartAd.ninja](StartAd.ninja), then replace the bracketed placeholders with the values from your SnoozeAd `gql` request:

in **Headers**:

* `Authorization: {YOUR_OAUTH}` <-- e.g. `OAuth abcdefghijklmnopqrstuvwxyz1234`
	
in **Data**:

* `channelID : "{YOUR_CHANNEL_ID}"` <-- e.g. `"1234567"`, note the key is `channelID`, rather than `targetChannelID` in `SnoozeAd`
* `lengthSeconds : {YOUR_AD_LENGTH}` <-- e.g. `90`, must match your ad length set in the Ads Manager. Note the lack of quotes
* `"sha256Hash": "{YOUR_SHA_HASH}"` <-- e.g. `"abcdefghijklmnopqrstuvwxyz123456abcdefghijklmnopqrstuvwxyz123456"`, there will be a different hash for each button; don't copy from one to the other
	
## Testing
You should be able to test each action now. If it doesn't work right away, troubleshoot with these steps in you API Ninja action:
1. Set **Response Shown** to `*`
2. Check **Save Response Shown to file** and set a **Response Filename**
3. Press the button again. Your response from Twitch will be saved to that file.
	
## Bonus
TheShiningOne put out [another video](https://www.youtube.com/watch?v=eRY6V1ssGkk) showing how to set Custom Tags via similar methods, as Twitch hasn't provided any documentation for that either.

## Contributor Links
* [ShiningOne on Twitch](https://www.twitch.tv/shiningone)
* [MaclunkeyCulkin on Twitch](https://www.twitch.tv/maclunkeyculkin)
