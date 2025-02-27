---
nav_title: Line
article_title: Line
alias: /partners/line/
description: "This article outlines the partnership between Braze and Line, one of the world's most popular instant messaging platforms."
page_type: partner
search_tag: Partner

---

# Line

> [Line](https://line.me/en/) is one of the world's most popular instant messaging platforms, used by hundreds of millions of monthly active users. Through this platform, brands can engage with their customers with rich and two-way messaging.

The Braze and Line integration allows you to leverage Braze webhooks, advanced segmentation, personalization, and triggering features to message your users in Line through the [Line messaging API](https://developers.line.biz/en/docs/messaging-api/overview/).

## Prerequisites

Line allows both promotional and non-promotional messaging to users as long your brand has secured users' consent. To send messages to users, you must meet one of two conditions:
- Users who have added your Line official account as a friend
- Users who haven't added your Line official account as a friend but have sent a message to your Line official account (excluding users who have blocked your LINE official account).
<br><br>

| Requirement | Description |
| ----------- | ----------- | 
| Line business account | A Line [official business account](https://www.linebiz.com/jp-en/) is required to take advantage of this partnership.<br><br>When sending Line messages, your messages will all be associated with your Line official account, resulting in users seeing your account name and page.|
| Messaging API channel | When enabling the use of the messaging API in the LINE [official account manager](https://developers.line.biz/en/docs/messaging-api/getting-started/#using-oa-manager), a messaging API channel is created. This will be the channel you use to communicate with your customers. |
| Channel access tokens |The [channel access token](https://developers.line.biz/en/docs/messaging-api/channel-access-tokens/) will allow you to send messages to users that have added your Line official account as a friend. This token can be found in the **Line Developer Console** under the **Messaging API** tab.
| Line user IDs | You will need to have users' Line IDs (this ID is different from users' usernames) to send messages on Line.<br><br>Once a user adds your Line official account as a friend, you can access the user's Line ID through Line's Users API. |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3  .reset-td-br-4}

## Integration

### Step 1: Collect customer Line IDs

To send messages in Line, you need to collect your users' Line IDs to identify your user and interact with them consistently. Line IDs are not the same as the user's Line usernames. Line IDs are generated by Line and can be used when interacting with Line's APIs.

Line IDs can be obtained using the Line [User ID API](https://developers.line.biz/en/reference/messaging-api/#get-follower-ids). This endpoint will return a list of Line IDs for all users who have friended your LINE official account or sent your account a message and have not blocked you. 

When making a GET request to the endpoint `https://api.line.me/v2/bot/followers/ids`, you will get the following:
```json
{
   "userIds":[
      "U4af4980629...",
      "U0c229f96c4...",
      "U95afb1d4df..."
   ],
   "next":"yANU9IA..."
}
```
Once you obtain a list of Line IDs, you will send them to Braze as a `line_id` custom attribute.

### Step 2: Send IDs to Braze as custom attributes

Coordinate and share this with your developers to send the `line_id` to Braze as [custom attributes]({{site.baseurl}}/user_guide/Data_and_Analytics/Custom_Data/Custom_Attributes/#custom-attributes).

### Step 3: Set your channel access token as a Content Block.

In Braze, navigate to **Templates & Media > Content Blocks Library > + Create Content Block** and create a Braze [Content Block]({{site.baseurl}}/user_guide/engagement_tools/templates_and_media/content_blocks/#content-blocks). Name this Content Block `LINE_Channel_AccessToken`. 

Next, paste your channel access token in the Content Block body, and save it.

![An image the Content Block showing the Content Block name, Liquid tag, and censored channel access token.][2]

Once you've set the channel access token inside a Content Block, you will be able to use the Line webhook template to send messages to users.

### Step 4: Select a webhook template

From **Templates & Media**, go to **Webhook Templates** and choose one of the following Line Messenger webhook templates: 

![A selection of available predesigned webhook templates.]({% image_buster /assets/img_archive/line_templates.png %}){: style="border:0px;"}

{% tabs %}
{% tab Line Text %}
The Line [text](https://developers.line.biz/en/docs/messaging-api/message-types/#text-messages) webhook template will allow you to send text-based messages that support emojis.

![The line messaging UI with two examples of what a text message will look like on their platform.]({% image_buster /assets/img_archive/line_text_type.png %}){: style="max-width:70%;border:0px;"}
{% endtab %}
{% tab Line Sticker %}
The Line [sticker](https://developers.line.biz/en/docs/messaging-api/message-types/#sticker-messages) template will allow you to send sticker messages. Stickers can be used to make your bot app more expressive and engaging for your users. 

To send a sticker, include the sticker's package ID and sticker ID in the message object. See the [list of available stickers](https://developers.line.biz/en/docs/messaging-api/sticker-list/) that can be sent with the Messaging API.

![The line messaging UI with several examples of what sticker messages look like. These examples include a bear celebrating, a rabbit giving a thumbs up, and a yellow duck.]({% image_buster /assets/img_archive/line_sticker_type.png %}){: style="max-width:70%;border:0px;"}
{% endtab %}
{% tab Line Image %}
The Line [image](https://developers.line.biz/en/docs/messaging-api/message-types/#image-messages) template allows you to send images to your Line users.

To send images, include URLs of the original image and a smaller preview image in the message object. The preview image is displayed in the chat, and the full image is opened when the image is tapped. Note that the URLs must use HTTPS over TLS 1.2 or later.

![The line messaging UI showing what an image message will look like on their platform.]({% image_buster /assets/img_archive/line_image_type.png %})
{% endtab %}
{% tab Line Carousel %}
The Line [carousel](https://developers.line.biz/en/docs/messaging-api/message-types/#carousel-template) template allows you to send messages with multiple column objects that users can cycle through. In addition to having buttons, you can also indicate in each column object a single action to be executed when a user taps anywhere in the image, title, or text area.

![The line messaging UI showing a carousel message. This message includes a swipable content box that includes an image, description, a reserve button, and a call button. ]({% image_buster /assets/img_archive/line_carousel_type.png %}){: style="max-width:70%;border:0px;"}
{% endtab %}
{% endtabs %}

### Step 5: Set up your webhook template

In your webhook template, provide a template name and add teams and tags, as necessary. Next, enter your message, sticker ID, or image depending on the Line template type you selected.

The custom attribute `Line ID` should be templated in the message body's `To:` field. If not, include the Line ID as a custom attribute. This can be done by using the blue and white + button in the corner of the **Request Body** box.

#### Previewing and Testing Your Webhook

Before you send your message, test your webhook. Make sure your Line ID is saved in Braze (or find it and test as a customized user), and use the preview to send the test message:

![Test tab in the Braze webhook builder that shows you can preview the message by sending it to an existing user.][3]

If you receive the message successfully, you can start to configure its delivery settings.

## Using this integration

Once set up, use this integration to target Line users. First, [create a segment][62] for all users for whom the `Line ID` exists as a custom attribute and turn on [analytics tracking][61] to track your Messenger subscription rates over time. 

![Segment filter "line_id" set to "is not blank".][63]

If you choose not to create a specific segment for Messenger subscribers, make sure to include a filter for `Line ID` existing to avoid errors.

You may also use other segmentation to target your Line campaigns and the rest of the campaign creation process, as is with any other campaign.

[1]: {% image_buster /assets/img_archive/line_channel_access_token.png %}
[2]: {% image_buster /assets/img_archive/line_content_block_token.png %}
[3]: {% image_buster /assets/img_archive/line_preview.png %}
[61]: {{site.baseurl}}/user_guide/data_and_analytics/your_reports/viewing_and_understanding_segment_data/#viewing-and-understanding-segment-data
[62]: {{site.baseurl}}/user_guide/engagement_tools/segments/creating_a_segment/#creating-a-segment
[63]: {% image_buster /assets/img_archive/line_segment.png %}
