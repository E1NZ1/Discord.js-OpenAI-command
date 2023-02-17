# Discord.js-OpenAI-command
Have you ever wanted to create an openAI command that tells Everything.

# Setup
First go to https://beta.openai.com/account/api-keys and get a api key.

# Coding code
First of all create a folder name functions where you will keep your function.
Then create a file openAI.js

On the top define the things.
```js
const { AttachmentBuilder } = require("discord.js");
const { Configuration,  OpenAIApi } = require("openai")
```

Good. Now after that below it let's create the configuration for us to connect to openai api.
```js
const openai = new OpenAIApi(
    new Configuration({
        apiKey: "", // your api key.
    })
);
```
Done. Wonderful. Now let's start the code first let's define params
```js
/**
* @param {string} prompt // the prompt
* @param {Object} interaction
* @return {string} responseText
*/
```

Now let's start the main code.
First let's create an function
```js
async function openAI(prompt, interaction) {}

module.exports = openAI
```

K. Now let's start coding
```js
try {
        const response = await openai.createCompletion({
            model: "text-davinci-003",
            prompt: prompt,
            max_tokens: 2000,
            temperature: 0.5,
        });
        
        const responseText = response.data.choices[0].text;
        
        if (responseText.length >= 2000) {
            const attachment = new AttachmentBuilder(
                Buffer.from(responseText, "utf-8"),
                {
                    name: "response.txt",
                }
            );
            await interaction.followUp({
                files: [attachment],
            })
        } else {
            await interaction.followUp({
                content: responseText,
            })
        }
        return responseText;
    } catch (err) {
        interaction.followUp(`OpenAI api may be down or an error occurred`)
        console.log(err.stack);
    }
}

module.exports = openAI;
```

Good Job.
Now let's create a command.

```js
const openAI = require("../../functions/openAI");
const timers = require("timers-promises")

module.exports = {
    name: "openai",
    description: "ask anything",
    options: [{
        name: "prompt",
        description: "the thing to ask",
        type: 3,
        required: true,
    }],
    run: async (client, interaction) => {
        await interaction.deferReply()
    openAI(interaction.options.getString("prompt"), interaction)
    },
};
```

And Now you have your own openAI command.
Bye👋.
