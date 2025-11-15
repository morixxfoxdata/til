# chat completion API について

OpenAI の提供する文章生成 API は「Completion API」と「Chat Completion API」の 2 つがある。「Completion API」はレガシーとなったため、基本的に「Chat Completion API」のみ使用される。

以下、使用例。

```json
{
  "model": "gpt-4o-mini",
  "messages": [
    { "role": "system", "content": "You are a helpful assistant." },
    { "role": "user", "content": "こんにちは。私の名前はJohnです。" }
  ]
}
```

また、`"role":"assistant"`として会話履歴を参照して渡すこともある。これは、Chat Completion API 自体がステートレスで ChatGPT のように会話履歴を元にして応答することはできない。

## 現在の料金

| Model     | Max Input tokens | Max output tokens | $/1M tokens  |
| --------- | ---------------- | ----------------- | ------------ |
| GPT-5.1   | 272,000          | 128,000           | Input $1.25  |
| -----     | --               | --                | Output $10   |
| GPT-5mini | 272,000          | 128,000           | Input $0.25  |
| -----     | --               | --                | Output $2.00 |
