# 逆引き Compose

## 画面サイズの取得

```kotlin
val configuration = LocalConfiguration.current

val widthInDp = configuration.screenWidthDp.dp
val heightInDp = configuration.screenHeightDp.dp
```

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

- Boxは純粋に箱
- Surface は Material Design の構成要素

- [Compose レイアウトの基本](https://developer.android.com/jetpack/compose/layouts/basics?hl=ja)

> 要素を別の要素の上に配置するには、Box を使用します。Box では、それに含まれる要素の特定の配置を設定することも可能です。

- [マテリアルデザイン｜No.01：Surfaces｜日本語解説](https://uidesign-dailylife.com/020-md-01-surfaces/)

> マテリアルデザインにおける「Surface」とは、「UIを構成するための概念」であり、マテリアルデザインは、全てこのSurfaceを組み合わせることによって構成されています。


- [Difference Between Surface and Box in Android Jetpack Compose](https://codingwithrashid.com/difference-between-surface-and-box-in-android-jetpack-compose/)
- [[Jetpack Compose] Surfaceを使おう](https://zenn.dev/kgmyshin/articles/89ed1d35292093)

