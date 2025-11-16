# API KEY どうする？

基本、.env ファイルをプロジェクト直下に用意して、

```python
from dotenv import load_dotenv
load_dotenv()
```

で、環境変数に.env 内の情報をロードできるらしい。
