# token 数を確認する

OpenAI で入力テキストが一体何 token なのかを確認するには

- Chat Completion API を叩けば確認できる（避けたい）
- Tokenizer を使用する(GPT4 までしか対応できていない)
- OpenAI が提供している Tiktoken(https://github.com/openai/tiktoken) を利用する

2025 年 11 月 15 日現在、tiktoken は gpt-5 まで対応している。
詳細は(https://github.com/openai/tiktoken/blob/main/tiktoken/model.py)を見ればいいと思う。
