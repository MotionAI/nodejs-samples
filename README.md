# Motion AI Node.js Module

We are incredibly excited to unveil Motion AI’s new “Node.js Module” which gives anyone the ability to easily code complex webservice-backed chatbots with no servers required.  This is made possible through a partnership with AWS Lambda.

Sign up for free on http://www.motion.ai and create a bot.  Once you've created your bot, drag and drop a Node.js module into your canvas to get started.

Our mission at Motion AI has always been simple - we want to make the process of training, structuring, and deploying chatbots as easy and as painless as possible.

Each Node.js Module function created through Motion AI is passed a payload object that contains metadata based on an end-user’s response to the bot. This data can be acted upon within the Node.js module to craft a bot response to be returned at the end of the function, which determines how your Motion AI bot responds.

This opens the doors to quickly creating bots that can do everything from image recognition to machine learning, without ever leaving your web browser or setting up a server.

# Trivia Bot Example
Demo our example Trivia Bot on: http://m.me/triviabotdemo
You may view triviabot.js in this repo for supporting code.

# Example Inbound Payload
Every function is passed an `event` JSON object, which contains context on the conversation.  Below is the data you may expect to receive from Motion AI:

    {
        "from":"string", // the end-user's identifier (may be FB ID, email address, Slack username etc, depends on bot type)
        "session":"string", // a unique session identifier
        "botId":"string", // the Motion AI ID of the bot
        "botType":"string", // the type of bot this is (FB, Slack etc)
        "customPayload":"string", // a developer-defined payload for carrying information
        "moduleId":"string", // the current Motion AI Module ID
        "reply":"string", // the end-user's reply that led to this module
        "result":"string" // any extracted data from the prior module, if applicable
    }

# Example Callback Payload
When you are finished processing the data, you must return a callback containing a JSON object that tells Motion AI how to respond to the end-user.

    // this is the object we will return to Motion AI in the callback
    var responseJSON = {
        "response": "Hello World!", // what the bot will respond with (more is appended below)
        "continue": false, // denotes that Motion AI should hit this module again, rather than continue further in the flow
        "customPayload": "", // working data to examine in future calls to this function to keep track of state
        "quickReplies": null, // a JSON object containing suggested/quick replies to display to the user
        "cards": null // a cards JSON object to display a carousel to the user (see docs)
    }

      // callback to return data to Motion AI (must exist, or bot will not work)
      callback(null, responseJSON);

# Included Libraries
Motion AI bundles several popular Node.js libraries that you can require() within your code.  See npmjs.com documentation for each library for usage instructions.
+ async
+ lodash
+ mongoose
+ mysql
+ q
+ redis
+ request
+ imagemagick
+ aws-sdk

# Sending Quick/Suggested Replies 
Your response JSON object may contain an array of strings to be displayed as Suggested Replies.  For example:
`responseJSON.quickReplies = ["Red","Blue","Green","Yellow"];`

# Sending Cards/Carousels
You may display rich carousels (depending upon your bot's platform) that contain buttons and informational cards.

    responseJSON.cards = [
        // Card 1
        {
          cardTitle: 'TestWebhookTitle', // Card Title
          cardSubtitle: 'TestWebhookSubtitle', // Card Subtitle
          cardImage: 'https://www.motion.ai/images/logo_molecules_gradient.png', // Source URL for image
          cardLink: 'https://www.motion.ai', // Click through URL
          buttons: [{
            buttonText: 'WebhookButton 1', // Button Call to Action
            buttonType: 'url', // either 'url' or 'module'
            target:  'https://dashboard.motion.ai'// Text to send to bot, or URL
          }]
        },
        // Card 2
        {
          cardTitle: 'TestWebhookTitle2',
          cardSubtitle: 'TestWebhookSubtitle2',
          cardImage: 'https://www.motion.ai/images/logo_molecules_gradient.png',
          cardLink: 'https://www.motion.ai',
          buttons: [{
            buttonText: 'WebhookButton 2',
            buttonType: 'url',
            target:  'https://dashboard.motion.ai'
          }]
        }
    ]
    
# Troubleshooting / FAQs
We recommend that you test your code locally during the development process, particularly if your function is large in scope.

**My bot is not responding, or shows an endless typing indicator**
This likely means that there is an error in your code somewhere.  Try testing the code locally to debug.  Your function *must* return a callback to Motion AI, and *must* exist within the `exports.handler` function. The other possibility is that your function exceeded the 10-second timeout limit we have imposed. If you would like to lift this limit, please contact us.

**I updated my Node.js module but the changes did not apply when I test my bot**
Please allow up to 2 minutes for code to fully deploy and propagate. Most changes are visible within 30-60 seconds, but network latency and larger codebases can cause delays.

**Can I add custom npm modules to my deployment package?**
At this time we do not support this.  If you feel we are missing an important library, please let us know! In the future we may offer the ability to bundle custom libraries.

# Help and Support
If you require assistance, please join our Slack community on http://slack.motion.ai or submit a support ticket on https://dashboard.motion.ai/help

We can't wait to see what you build!
