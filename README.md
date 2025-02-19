# Creating a ChatBot with Open-AIs text-davinci-002

So I have been using GitHub copilot for a while now. GitHub Copilot is a Plugin for VSCode/JB IDEs/NVIM that provides
you with intelligent code completion, suggestions and in my opinion the next big thing in Software Development in
general.

I was always really interested in how the whole AI Suggestion works and how it could be used in my own projects.

![test](https://copilot.github.com/diagram.png)

While looking into it, I came across the [Open-AI Playground](https://beta.openai.com/examples/default-text-to-command),
a playground for Open-AI's text-davinci-002 model.

### What is Open-AI's text-davinci-002 API?

text-davinci-002 is a model that can be trained to generate text from a given input.

It also provides an API to interact with the model which is actually quite easy to use.

```js
const { Configuration, OpenAIApi } = require("openai");

const configuration = new Configuration({
  apiKey: process.env.OPENAI_API_KEY,
});
const openai = new OpenAIApi(configuration);

const response = await openai.createCompletion("text-davinci-002", {
  prompt: "Hey how are you?\n", // question for the ai goes here
  temperature: 0, // 0 means no randomness and usually the best result
  max_tokens: 100, 
  top_p: 1.0,
  frequency_penalty: 0.2,
  presence_penalty: 0.0,
  stop: ["\n"],
});
```

### Creating the ChatBot

So I decided to create a ChatBot that can be used to interact with the AI. Developing with GitHubs Copilot I already
noticed, that Context is always very important and helps the AI to understand what kind of response he should give.

So first thing I do is configure the AI! How do you ask? **WITH CLEAR TEXT**!

```js
function conversationContext(aiName, attributes) {
  return `\n
    The following is a conversation with an AI. The AI is ${attributes}.
    \n
    Human:Hello
    \n
    ${aiName}:Hi, I am an AI. Whats your question?
    \n`
}
```

Now we will feed this context to the AI, which, with more context, will give us a better response.

```js
const promt = `${conversation}Human:${question}
    \n
    ${aiName}:`
```

Here we just want want the AI has to say, like:

```json
Human:Hello, who are you?
AI:${responseFromTheAI}
```

### The Final Bot

Wrapping that all up into a nice React Application and the Bot is ready to go! Below I have some examples.


![test1](https://i.imgur.com/8DMtllb.png)
![test2](https://i.imgur.com/N2E4x6N.png)

The Chatbot is hosted on Netlify and the source code is available on [Github](https://github.com/Wetwer/davinci-chatbot)

[Chatbot on Netlify](https://musical-dodol-0cff4a.netlify.app/)

Have fun, and I hope will find it useful in maybe finding your next Project Idea!
