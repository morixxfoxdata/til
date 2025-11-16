# LangChain

LangChain は LLM を用いたアプリケーション開発に特化した Python のライブラリである。

LangChain は複数のコンポーネントからなる。

langchain-core(核となる抽象基底クラス)、partners(openai や anthropic)など。

基本的には langchain-core に加えて必要最小限のパッケージをインストールして使用する。
たとえば LangChain で OpenAI の Chat Completion API を利用する場合は、langchain-core と langchain-openai をインストールする。
