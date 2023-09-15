# Jetpack Compose の高度な状態と副作用

学習メモ

https://developer.android.com/codelabs/jetpack-compose-advanced-state-side-effects

##  LaunchedEffect

- Compoisionに入ったときにコルーチンを起動する。
- Composition の退場時にコルーチンはキャンセルされる。
- 引数 key に値を渡すことで、キーの変化のたびにコルーチンを実行できる。
- keyに Unitを渡すと、1回目の Composition (Composition開始時) でだけコルーチンを起動することができる。
- コルーチンの中からコールバック関数を使って上位に通知を行うときは、コールバック関数を `rememberUpdateState` で管理する。

## rememberCoroutineScope

コールバック関数の先でコルーチンを実行するときに使う

呼び出し元でコルーチンスコープを管理する。

子の Composable にコールバック関数を引数で渡す。コールバック関数の中でコルーチンスコープに基づいたコルーチンの呼び出しを行う。

## 状態ホルダーの作成

状態ホスティング (巻き上げ) ... 上位のオブジェクトに状態を預けること

変数が多い = 引数が増える と管理がめんどくさくなる → 状態ホルダーを作ってまとめる

- 状態ホルダークラスを作る
- 状態ホルダーを記憶するための `Composable` な `remember` 関数を作る
- Activityの再生成に備えて状態ホルダーをカスタムセーバに対応させる

- 呼び出し側で状態ホルダーの変数を持ち、子の Composeに値を渡す
- 呼び出される側は渡された状態ホルダーを参照して Compose を実行する

### snapshotFlow

State を Flow に変換する。
LaunchedEffect の中で Flowに変換して、 collectするのが定石?


## DisposableEvent

`ライフサイクルの変化をカスタムビューに通知する`ために使われるのが主な使われ方。

新しいライブラリでもっと簡単になったっぽい (まだα)
[ComposeでLifecycleを監視する(2023年9月バージョン)](https://star-zero.medium.com/compose%E3%81%A7lifecycle%E3%82%92%E7%9B%A3%E8%A6%96%E3%81%99%E3%82%8B-2023%E5%B9%B49%E6%9C%88%E3%83%90%E3%83%BC%E3%82%B8%E3%83%A7%E3%83%B3-8aae9b277f5c)


## produceState

「Compose 外の状態」を Compose の State に変換する

コルーチンを実行 → 状態が変化 → UiState (独自のデータクラス) → 再 Compose が発生

## derivedStateOf

derive = 引き出す

他の State の値をもとにして新しく作られる(引き出された) State。
パフォーマンスの改善に役立つ。

Craneアプリの場合、スクロール位置の変化は操作時に大量に発生するが、それに伴って起きる「ボタンの表示・非表示の変更」はそれほど発生しない。

スクロール位置をもとに描画を行なっても動作に違いは見られないかもしれないが、パフォーマンスに影響することが考えられる。そのようなときに derivedStateは役に立つ。

[When should I use derivedStateOf?](https://medium.com/androiddevelopers/jetpack-compose-when-should-i-use-derivedstateof-63ce7954c11b)

> The answer to this question is derivedStateOf {} should be used when
> your state or key is changing more than you want to update your UI






## メモ
- collectAsStateWithLifecycle
  - [collectAsStateWithLifecycleが追加されたぞ](https://qiita.com/dosukoi_android/items/e8bbaa662c52b8e1cc20)
  
  
