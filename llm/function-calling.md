# Function calling

API を叩く際に tool として python 関数を渡すことができる機能。ただ LLM 自体が Python を実行できるわけではないので、返答として「この値を引数として渡して関数 A を実行したい」といってくれる。その返答を元にこちらで実行する。

たとえば以下のような`get_current_weather`関数があるとする。

```python
import json

def get_current_weather(location, unit="fahrenheit"):
    if "tokyo" in location.lower():
        return json.dumps({"location": "Tokyo", "temperature": "10", "unit": unit})
    elif "san francisco" in location.lower():
        return json.dumps({"location": "San Francisco", "temperature": "72", "unit": unit})
    elif "paris" in location.lower():
        return json.dumps({"location": "Paris", "temperature": "22", "unit": unit})
    else:
        return json.dumps({"location": location, "temperature":"Unknown"})
```

この関数を呼ぶために、tools という名前で説明やパラメータを定義する。

```python
tools = [
    {
        "type": "function",
        "function": {
            "name": "get_current_weather",
            "description":"Get the current weather in a given location",
            "parameters": {
                "type": "object",
                "properties": {
                    "location":{
                        "type": "string",
                        "description": "The city and state, e.g. SanFrancisco, CA"
                    },
                    "unit": {
                        "type": "string",
                        "enum":["celsius", "fahrenheit"]
                    },
                },
                "required": ["location"],
            },
        },
    }
]
```

先ほど同様、chat completion api を呼ぶ際に tools を渡す。

```python
from openai import OpenAI

client = OpenAI()

messages = [
    {"role":"user", "content":"東京の天気はどうですか？"},
]

response = client.chat.completions.create(
    model="gpt-4o",
    messages=messages,
    tools=tools,
)
print(response.to_json(indent=2))
```

```
# 出力
{
  "id": "chatcmpl-Cc9johMPbciDKBbagSYs3fbUWVyMY",
  "choices": [
    {
      "finish_reason": "tool_calls",
      "index": 0,
      "logprobs": null,
      "message": {
        "content": null,
        "refusal": null,
        "role": "assistant",
        "annotations": [],
        "tool_calls": [
          {
            "id": "call_09355WDdtWcwYumhJJcK7iEC",
            "function": {
              "arguments": "{\"location\":\"Tokyo, Japan\",\"unit\":\"celsius\"}",
              "name": "get_current_weather"
            },
            "type": "function"
          }
        ]
      }
    }
  ],
  "created": 1763209932,
  "model": "gpt-4o-2024-08-06",
  "object": "chat.completion",
  "service_tier": "default",
  "system_fingerprint": "fp_b1442291a8",
  "usage": {
    "completion_tokens": 22,
    "prompt_tokens": 82,
    "total_tokens": 104,
    "completion_tokens_details": {
      "accepted_prediction_tokens": 0,
      "audio_tokens": 0,
      "reasoning_tokens": 0,
      "rejected_prediction_tokens": 0
    },
    "prompt_tokens_details": {
      "audio_tokens": 0,
      "cached_tokens": 0
    }
  }
}
```

LLM が、get_current_weather に対して東京を引数として渡すことを望んでいることが応答から読み取れる。また、message 内部の content が null になっている。

messages にこの履歴を append して履歴を残す。

```python
response_message = response.choices[0].message
messages.append(response_message.to_dict())
```

LLM が呼びたい関数を読んで、関数を実行し、関数の実行結果を会話履歴に追加する。この時の role は tool になる。

```python
available_functions = {
    "get_current_weather": get_current_weather,
}

for tool_call in response_message.tool_calls:
    function_name = tool_call.function.name
    function_to_call = available_functions[function_name]
    function_args = json.loads(tool_call.function.arguments)
    function_response = function_to_call(
        location=function_args.get("location"),
        unit=function_args.get("unit"),
    )
    messages.append(
        {
            "tool_call_id":tool_call.id,
            "role":"tool",
            "name":function_name,
            "content":function_response,
        }
    )

print(json.dumps(messages, ensure_ascii=False, indent=2))
```

ここまでで、tool を付与して質問、LLM から Function calling の要望、こちらで要望通りに関数を実行して結果を messages に追加をした。結構世話をした。

この後、この messages を渡せばちゃんと返してくれる。

```python
second_response = client.chat.completions.create(
    model="gpt-4o",
    messages=messages,
)
print(second_response.to_json(indent=2))
```

出力

```
{
  "id": "chatcmpl-CcA82fVtZjtkUETCaCnvlYcQHECaQ",
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "logprobs": null,
      "message": {
        "content": "東京の現在の気温は約10度です。",
        "refusal": null,
        "role": "assistant",
        "annotations": []
      }
    }
  ],
  "created": 1763211434,
  "model": "gpt-4o-2024-08-06",
  "object": "chat.completion",
  "service_tier": "default",
  "system_fingerprint": "fp_b1442291a8",
  "usage": {
    "completion_tokens": 12,
    "prompt_tokens": 66,
    "total_tokens": 78,
    "completion_tokens_details": {
      "accepted_prediction_tokens": 0,
      "audio_tokens": 0,
      "reasoning_tokens": 0,
      "rejected_prediction_tokens": 0
    },
    "prompt_tokens_details": {
      "audio_tokens": 0,
      "cached_tokens": 0
    }
  }
}
```
