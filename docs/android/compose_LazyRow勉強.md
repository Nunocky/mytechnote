# LazyRow の勉強

`LazyRow` で横方向スクロールするアイテムリストを作る

- データを用意する
  - タイトルと画像 URL 10個くらい
  - 画像 https://picsum.photos/ から適当に

- item と items (見出しを簡単に作れる)
- 左右パディングの問題 (contentPadding)
- アイテム間の隙間 (horizontalArrangement)
- タップ時の処理。コールバックでタップされたアイテムの情報を渡す
- rememberLazyListState の使い方 (firstVisibleItemIndex, firstVisibleItemScrollOffset, etc.)
  - 「先頭にスクロール」ボタンを追加 ... `derivedState` の活用
- animateScrollToItem ... コルーチンの実行。coroutineScopeを使う
- タップされたアイテムを中央に (https://qiita.com/replicant101/items/9c87dd4f30d04aa0b239)
