import Image from '@theme/IdealImage';

# Langfuse - Logging LLM Input/Output

LangFuse is open Source Observability & Analytics for LLM Apps
Detailed production traces and a granular view on quality, cost and latency

<Image img={require('../../img/langfuse.png')} />

:::info
We want to learn how we can make the callbacks better! Meet the LiteLLM [founders](https://calendly.com/d/4mp-gd3-k5k/berriai-1-1-onboarding-litellm-hosted-version) or
join our [discord](https://discord.gg/wuPM9dRgDw)
::: 

## Pre-Requisites
Ensure you have run `pip install langfuse` for this integration
```shell
pip install langfuse>=2.0.0 litellm
```

## Quick Start
Use just 2 lines of code, to instantly log your responses **across all providers** with Langfuse
<a target="_blank" href="https://colab.research.google.com/github/BerriAI/litellm/blob/main/cookbook/logging_observability/LiteLLM_Langfuse.ipynb">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a>

Get your Langfuse API Keys from https://cloud.langfuse.com/
```python
litellm.success_callback = ["langfuse"]
litellm.failure_callback = ["langfuse"] # logs errors to langfuse
```
```python
# pip install langfuse 
import litellm
import os

# from https://cloud.langfuse.com/
os.environ["LANGFUSE_PUBLIC_KEY"] = ""
os.environ["LANGFUSE_SECRET_KEY"] = ""
# Optional, defaults to https://cloud.langfuse.com
os.environ["LANGFUSE_HOST"] # optional

# LLM API Keys
os.environ['OPENAI_API_KEY']=""

# set langfuse as a callback, litellm will send the data to langfuse
litellm.success_callback = ["langfuse"] 
 
# openai call
response = litellm.completion(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "user", "content": "Hi 👋 - i'm openai"}
  ]
)
```

## Advanced
### Set Custom Generation names, pass metadata

Pass `generation_name` in `metadata`

```python
import litellm
from litellm import completion
import os

# from https://cloud.langfuse.com/
os.environ["LANGFUSE_PUBLIC_KEY"] = ""
os.environ["LANGFUSE_SECRET_KEY"] = ""


# OpenAI and Cohere keys 
# You can use any of the litellm supported providers: https://docs.litellm.ai/docs/providers
os.environ['OPENAI_API_KEY']=""

# set langfuse as a callback, litellm will send the data to langfuse
litellm.success_callback = ["langfuse"] 
 
# openai call
response = completion(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "user", "content": "Hi 👋 - i'm openai"}
  ],
  metadata = {
    "generation_name": "litellm-ishaan-gen", # set langfuse generation name
    # custom metadata fields
    "project": "litellm-proxy" 
  }
)
 
print(response)

```

### Set Custom Trace ID, Trace User ID and Tags

Pass `trace_id`, `trace_user_id` in `metadata`

```python
import litellm
from litellm import completion
import os

# from https://cloud.langfuse.com/
os.environ["LANGFUSE_PUBLIC_KEY"] = ""
os.environ["LANGFUSE_SECRET_KEY"] = ""

os.environ['OPENAI_API_KEY']=""

# set langfuse as a callback, litellm will send the data to langfuse
litellm.success_callback = ["langfuse"] 

# set custom langfuse trace params and generation params
response = completion(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "user", "content": "Hi 👋 - i'm openai"}
  ],
  metadata={
      "generation_name": "ishaan-test-generation",  # set langfuse Generation Name
      "generation_id": "gen-id22",                  # set langfuse Generation ID 
      "trace_user_id": "user-id2",                  # set langfuse Trace User ID
      "session_id": "session-1",                    # set langfuse Session ID
      "tags": ["tag1", "tag2"]                      # set langfuse Tags
      "trace_id": "trace-id22",                     # set langfuse Trace ID
      ### OR ### 
      "existing_trace_id": "trace-id22",                     # if generation is continuation of past trace. This prevents default behaviour of setting a trace name
  },
)

print(response)

```

### Use LangChain ChatLiteLLM + Langfuse
Pass `trace_user_id`, `session_id` in model_kwargs
```python
import os
from langchain.chat_models import ChatLiteLLM
from langchain.schema import HumanMessage
import litellm

# from https://cloud.langfuse.com/
os.environ["LANGFUSE_PUBLIC_KEY"] = ""
os.environ["LANGFUSE_SECRET_KEY"] = ""

os.environ['OPENAI_API_KEY']=""

# set langfuse as a callback, litellm will send the data to langfuse
litellm.success_callback = ["langfuse"] 

chat = ChatLiteLLM(
  model="gpt-3.5-turbo"
  model_kwargs={
      "metadata": {
        "trace_user_id": "user-id2", # set langfuse Trace User ID
        "session_id": "session-1" ,  # set langfuse Session ID
        "tags": ["tag1", "tag2"] 
      }
    }
  )
messages = [
    HumanMessage(
        content="what model are you"
    )
]
chat(messages)
```

## Redacting Messages, Response Content from Langfuse Logging 

Set `litellm.turn_off_message_logging=True` This will prevent the messages and responses from being logged to langfuse, but request metadata will still be logged.

## Troubleshooting & Errors
### Data not getting logged to Langfuse ? 
- Ensure you're on the latest version of langfuse `pip install langfuse -U`. The latest version allows litellm to log JSON input/outputs to langfuse

## Support & Talk to Founders

- [Schedule Demo 👋](https://calendly.com/d/4mp-gd3-k5k/berriai-1-1-onboarding-litellm-hosted-version)
- [Community Discord 💭](https://discord.gg/wuPM9dRgDw)
- Our numbers 📞 +1 (770) 8783-106 / ‭+1 (412) 618-6238‬
- Our emails ✉️ ishaan@berri.ai / krrish@berri.ai
