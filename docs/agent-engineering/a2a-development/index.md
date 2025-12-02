---
title: A2A Development
layout: default
parent: Agent Engineering
---
Google Adk
[Python Quickstart Tutorial: Building an A2A Agent](https://a2a-protocol.org/latest/tutorials/python/1-introduction/#tutorial-sections)

[Quickstart: Exposing a remote agent via A2A](https://google.github.io/adk-docs/a2a/quickstart-exposing/)

[Quickstart: Consuming a remote agent via A2A](https://google.github.io/adk-docs/a2a/quickstart-consuming/)


```

from google.adk.agents.remote_a2a_agent import AGENT_CARD_WELL_KNOWN_PATH
from google.adk.agents.remote_a2a_agent import RemoteA2aAgent

root_agent = RemoteA2aAgent(
    name="hello_world_agent",
    description=(
        "Helpful assistant that can roll dice and check if numbers are prime."
    ),
    agent_card=f"http://localhost:8001/{AGENT_CARD_WELL_KNOWN_PATH}",
)
```
This error confirms that the `RemoteA2aAgent` class **does not accept** a `model` argument.

You likely added `model="gemini-2.0-flash"` while trying to fix the previous "Missing key" error. However, a `RemoteA2aAgent` is just a "connector" or proxy that talks to an agent running elsewhere (at the URL you provided). It doesn't run the model itself, so it doesn't know what to do with that parameter.

### The Fix

Open `/Users/clark/agent-development/hos-a2a/hos01-v3/a2a_root/agent.py` and **remove the `model` line** entirely.

**Incorrect Code (What you have now):**

```python
root_agent = RemoteA2aAgent(
    name="hello_world_agent",
    description="...",
    model="gemini-2.0-flash",  # <--- DELETE THIS LINE
    agent_card=f"http://localhost:8001/{AGENT_CARD_WELL_KNOWN_PATH}",
)
```

**Correct Code:**

```python
root_agent = RemoteA2aAgent(
    name="hello_world_agent",
    description=(
        "Helpful assistant that can roll dice and check if numbers are prime."
    ),
    # The 'model' is defined in the agent running on port 8001, not here.
    agent_card=f"http://localhost:8001/{AGENT_CARD_WELL_KNOWN_PATH}",
)
```

### Why this happens

Think of `RemoteA2aAgent` as a **Remote Control**.

  * The **Remote Control** (your `RemoteA2aAgent`) doesn't need to know the internal specs of the TV (the Model). It just needs to know where to point the signal (the `agent_card` URL).
  * The **TV** (the agent running on port 8001) is the one that actually has the "screen" (the Gemini Model) inside it.

Once you remove that line, you will likely return to the authentication/key issue. At that point, use the `.env` fix discussed in the previous step (setting `GOOGLE_GENAI_USE_VERTEXAI=FALSE`).