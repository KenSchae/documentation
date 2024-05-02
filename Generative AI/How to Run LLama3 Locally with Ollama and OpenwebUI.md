I’m a big fan of Llama. Meta releasing their LLM open source is a net benefit for the tech community at large, and their permissive license allows most medium and small businesses to use their LLMs with little to no restrictions (within the bounds of the law, of course). Their latest release is Llama 3, which has been highly anticipated.

Llama 3 comes in two sizes: 8 billion and 70 billion parameters. This kind of model is trained on a massive amount of text data and can be used for a variety of tasks, including generating text, translating languages, writing different kinds of creative content, and answering your questions in an informative way. Meta touts Llama 3 as one of the best open models available, but it is still under development. Here’s the 8B model benchmarks when compared to Mistral and Gemma (according to Meta).

[![Benchmarks](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fax9r9z2w2zghv81grbh7.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fax9r9z2w2zghv81grbh7.png)

This begs the question: how can I, the regular individual, run these models locally on my computer?

## [](https://dev.to/timesurgelabs/how-to-run-llama-3-locally-with-ollama-and-open-webui-297d#getting-started-with-ollama)Getting Started with Ollama

That’s where [Ollama](https://ollama.com/) comes in! Ollama is a free and open-source application that allows you to run various large language models, including Llama 3, on your own computer, even with limited resources. Ollama takes advantage of the performance gains of llama.cpp, an open source library designed to allow you to run LLMs locally with relatively low hardware requirements. It also includes a sort of package manager, allowing you to download and use LLMs quickly and effectively with just a single command.

The first step is [installing Ollama](https://ollama.com/download). It supports all 3 of the major OSes, with [Windows being a “preview”](https://ollama.com/blog/windows-preview) (nicer word for beta).

Once this is installed, open up your terminal. On all platforms, the command is the same.  

```
ollama run llama3
```

Wait a few minutes while it downloads and loads the model, and then start chatting! It should bring you to a chat prompt similar to this one.  

```
ollama run llama3
>>> Who was the second president of the united states?
The second President of the United States was John Adams. He served from 1797 to 1801, succeeding
George Washington and being succeeded by Thomas Jefferson.

>>> Who was the 30th?
The 30th President of the United States was Calvin Coolidge! He served from August 2, 1923, to March 4,
1929.

>>> /bye
```

You can chat all day within this terminal chat, but what if you want something more ChatGPT-like?

## [](https://dev.to/timesurgelabs/how-to-run-llama-3-locally-with-ollama-and-open-webui-297d#open-webui)Open WebUI

Open WebUI is an extensible, self-hosted UI that runs entirely inside of [Docker](https://docs.docker.com/desktop/). It can be used either with Ollama or other OpenAI compatible LLMs, like LiteLLM or my own [OpenAI API for Cloudflare Workers](https://github.com/chand1012/openai-cf-workers-ai).

Assuming you already have [Docker](https://docs.docker.com/desktop/) and Ollama running on your computer, [installation](https://docs.openwebui.com/getting-started/#quick-start-with-docker-) is super simple.  

```
docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main
```

The simply go to [http://localhost:3000](http://localhost:3000/), make an account, and start chatting away!

[![OpenWebUI Example](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Frdi1d35zh09s78o8vqvb.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Frdi1d35zh09s78o8vqvb.png)

If you didn’t run Llama 3 earlier, you’ll have to pull some models down before you can start chatting. Easiest way to do this is to click the settings icon after clicking your name in the bottom left.

[![Settings](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Ftqyetksyn0y4a0p12ylu.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Ftqyetksyn0y4a0p12ylu.png)

Then clicking on “models” on the left side of the modal, then pasting in a name of a model from the [Ollama registry](https://ollama.com/models). Here are some models that I’ve used that I recommend for general purposes.

- `llama3`
- `mistral`
- `llama2`

[![Models Setting Page](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Ftxc581jf4w3xszymjfbg.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Ftxc581jf4w3xszymjfbg.png)

## [](https://dev.to/timesurgelabs/how-to-run-llama-3-locally-with-ollama-and-open-webui-297d#ollama-api)Ollama API

If you want to integrate Ollama into your own projects, Ollama offers both its own API as well as an OpenAI Compatible API. The APIs automatically load a locally held LLM into memory, run the inference, then unload after a certain timeout. You do have to pull whatever models you want to use before you can run the model via the API, which can easily be done via the command line.  

```
ollama pull mistral
```

### [](https://dev.to/timesurgelabs/how-to-run-llama-3-locally-with-ollama-and-open-webui-297d#ollama-api)Ollama API

Ollama has their own API available, which also has a [couple of SDKs](https://github.com/ollama/ollama?tab=readme-ov-file#libraries) for Javascript and Python.

Here is how you can do a simple text generation inference with the API.  

```
curl http://localhost:11434/api/generate -d '{
  "model": "mistral",
  "prompt":"Why is the sky blue?"
}'
```

And here’s how you can do a Chat generation inference with the API.  

```
curl http://localhost:11434/api/chat -d '{
  "model": "mistral",
  "messages": [
    { "role": "user", "content": "why is the sky blue?" }
  ]
}'
```

Replace the `model` parameter with whatever model you want to use. See the [official API docs](https://github.com/ollama/ollama/blob/main/docs/api.md) for more information.

### [](https://dev.to/timesurgelabs/how-to-run-llama-3-locally-with-ollama-and-open-webui-297d#openai-compatible-api)OpenAI Compatible API

You can also use Ollama as a drop in replacement (depending on use case) with the OpenAI libraries. Here’s an example from [their documentation](https://github.com/ollama/ollama/blob/main/docs/openai.md).  

```
# Python
from openai import OpenAI

client = OpenAI(
    base_url='http://localhost:11434/v1/',

    # required but ignored
    api_key='ollama',
)

chat_completion = client.chat.completions.create(
    messages=[
        {
            'role': 'user',
            'content': 'Say this is a test',
        }
    ],
    model='mistral',
)
```

This also works for Javascript.  

```
// Javascript
import OpenAI from 'openai'

const openai = new OpenAI({
  baseURL: 'http://localhost:11434/v1/',

  // required but ignored
  apiKey: 'ollama',
})

const chatCompletion = await openai.chat.completions.create({
  messages: [{ role: 'user', content: 'Say this is a test' }],
  model: 'llama2',
})
```

## [](https://dev.to/timesurgelabs/how-to-run-llama-3-locally-with-ollama-and-open-webui-297d#conclusion)Conclusion

The release of Meta's Llama 3 and the open-sourcing of its Large Language Model (LLM) technology mark a major milestone for the tech community. With these advanced models now accessible through local tools like Ollama and Open WebUI, ordinary individuals can tap into their immense potential to generate text, translate languages, craft creative writing, and more. Furthermore, the availability of APIs enables developers to seamlessly integrate LLMs into new projects or enhance existing ones. Ultimately, the democratization of LLM technology through open-source initiatives like Llama 3 unlocks a vast realm of innovative possibilities and fuels creativity in the tech industry.