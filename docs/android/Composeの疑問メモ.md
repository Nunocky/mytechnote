# Composeの疑問メモ

## lifecycleOwner, lifecycleState の取得

```kotlin
val lifecycleOwner = LocalLifecycleOwner.current
val lifecycleState by lifecycleOwner.lifecycle.currentStateAsState()
```

## contextの取得

context

```kotlin
val context = LocalContext.current
```

activity

```kotlin
val activity = LocalContext.current as Activity
```


## Box と Surface の使い分け

- [Compose レイアウトの基本](https://developer.android.com/jetpack/compose/layouts/basics?hl=ja)

> 要素を別の要素の上に配置するには、Box を使用します。Box では、それに含まれる要素の特定の配置を設定することも可能です。

> ほとんどの場合、これらのビルディング ブロックだけで済みます。独自のコンポーズ可能な関数を作成することで、こうしたレイアウトを組み合わせ、アプリに適した、より手の込んだレイアウトにできます。

- [Difference Between Surface and Box in Android Jetpack Compose](https://codingwithrashid.com/difference-between-surface-and-box-in-android-jetpack-compose/)
- [[Jetpack Compose] Surfaceを使おう](https://zenn.dev/kgmyshin/articles/89ed1d35292093)

## 画面サイズの取得

```kotlin
val configuration = LocalConfiguration.current

val widthInDp = configuration.screenWidthDp.dp
val heightInDp = configuration.screenHeightDp.dp
```
