# LangChain

- [langchain\-ai/langchain: 🦜🔗 Build context\-aware reasoning applications](https://github.com/langchain-ai/langchain)
- [Introduction \| 🦜️🔗 LangChain](https://python.langchain.com/docs/get_started/introduction/)

- [LangChain \- Wikipedia](https://ja.wikipedia.org/wiki/LangChain)

> LangChain（ラングチェイン）は、大規模言語モデル（LLM）を使ったアプリケーションソフトウェアの作成を簡素化するように設計されたフレームワークである。言語モデル統合フレームワークとして、LangChainのユースケースは、文書解析や要約、チャットボット、コード解析など、一般的な言語モデルのユースケースとほぼ重なっている。


- [LangChainの概要と使い方｜サクッと始めるプロンプトエンジニアリング【LangChain / ChatGPT】](https://zenn.dev/umi_mori/books/prompt-engineer/viewer/langchain_overview)
- [LangChain を Python で使う \| Hakky Handbook](https://book.st-hakky.com/data-science/langcain-in-python/)

## 機能

### Model I/O

> Model I/Oとは「OpenAIをはじめとした様々な言語モデル・チャットモデル・エンべディングモデルを切り替えたり、組み合わせたりすることができる機能」です。

### Retrieval

> Retrievalとは「言語モデルが学習していない事柄に関して、外部データを用いて、回答を生成するための機能」です。
> 
> また、LangChainは外部データを読み込むための機能も用意しています。
> 
> 現在、132個のデータ読み込みに対応しています。
> 
> 馴染みがあるデータとしては「PDF、CSV、PowerPoint、Word、HTML、Markdown、Email、URL」などがあり、SaaSのサービスとしては「Notion、Figma、EverNote、Google Drive、Slack、YouTube」があります。

### Chains

> Chainsは「複数のプロンプト入力を実行する機能」です。

### Memory

> Memoryとは「ChainsやAgentsの内部における状態保持をする機能」です。

### Agents

> Agentsとは「言語モデルに渡されたツールを用いて、モデル自体が、次にどのようなアクションを取るかを決定・実行・観測・完了するまで繰り返す機能」です。

### Callbacks

> Callbacksとは「大規模言語モデルのアプリケーションのロギング、モニタリング、非同期処理などを効率的に管理する機能」です。

## LangChainの全体構成

- LangChainライブラリ
  - langchain-core
  - langchain-community
  - langchain
- LangChain Templates
- LangServe
- LangSmith


## langchain langsmith

https://python.langchain.com/docs/langsmith/walkthrough/

langchain-cli というのをインストール

> LangChain CLI は、LangChainテンプレートや他のLangServeプロジェクトを操作する場合に役立ちます。


```py
llm = ChatOpenAI(model_name="gpt-3.5-turbo", temperature=0)

prompt = ChatPromptTemplate.from_messages([
    ("system", "あなたは優秀な校正者です。"),
    ("user", "次の文章に誤字があれば訂正してください。\n{sentences_before_check}")
])

chain = prompt | llm
```


langsmithの API key

```
    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
```

