# OpenAI

- ストリーミング対応


---

#### asyncio

pythonの機能としてあるらしい。使い方がわからない。

[asyncio \-\-\- 非同期 I/O — Python 3\.12\.3 ドキュメント](https://docs.python.org/ja/3/library/asyncio.html)


```python
import asyncio

async def main():
    print("Hello ...")
    await asyncio.sleep(1)
    print("... World!")

asyncio.run(main())
```

