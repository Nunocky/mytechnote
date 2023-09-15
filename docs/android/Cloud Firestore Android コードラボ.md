# Firebase学習 (friendlyeats-androidのコードを読む)

[2023-08-31 11:07]

[Cloud Firestore Android コードラボ](https://firebase.google.com/codelabs/firestore-android?hl=ja) のコードを読む

## 気にしているところ

- Initializer でモジュールを初期化している
- Query, Snapshotなどの型
- firestoreと連携するための deta class
- ログインの実際の処理

## ファイル

```text
                `-- fireeats
                    |-- FilterDialogFragment.kt
                    |-- Filters.kt
                    |-- MainActivity.kt
                    |-- MainFragment.kt
                    |-- RatingDialogFragment.kt
                    |-- RestaurantDetailFragment.kt
                    |-- adapter
                    |   |-- FirestoreAdapter.kt
                    |   |-- RatingAdapter.kt
                    |   `-- RestaurantAdapter.kt
                    |-- model
                    |   |-- Rating.kt
                    |   `-- Restaurant.kt
                    |-- util
                    |   |-- AuthInitializer.kt
                    |   |-- FirestoreInitializer.kt
                    |   |-- RatingUtil.kt
                    |   `-- RestaurantUtil.kt
                    `-- viewmodel
                        `-- MainActivityViewModel.kt

```

### ✔ MainActivity.kt

ツールバーとナビゲーションの設定をしているだけ。

### ★★★ MainFragment.kt

メイン画面。
firebaseへのアクセスが多いのでよく読む必要がある

- サインイン・サインアウト
- レストラン一覧の取得と表示
- ランダムにレストランを追加する機能


#### サインイン

ビルトインのログインフォームを呼んでいる

```kotlin
    private val signInLauncher = registerForActivityResult(
        FirebaseAuthUIActivityResultContract()
    ) { result -> this.onSignInResult(result) }


    private fun startSignIn() {
        // Sign in with FirebaseUI
        val intent = AuthUI.getInstance().createSignInIntentBuilder()
                .setAvailableProviders(listOf(AuthUI.IdpConfig.EmailBuilder().build()))
                .setIsSmartLockEnabled(false)
                .build()

        signInLauncher.launch(intent)
        viewModel.isSigningIn = true
    }

```

レイアウトのカスタマイズ、というか AuthUI の実装でアプリに合わせた UI にすることは可能っぽい。
- https://firebaseopensource.com/projects/firebase/firebaseui-android/auth/readme/
- https://firebase.google.com/docs/auth/android/firebaseui?hl=ja

サインイン状態は `Firebase.auth.currentUser` で把握できる。

結果は `onSignInResult` で受けている。

#### レストラン一覧の表示

"restaurants" のコレクションを取得し、並べかえと表示上限を設定したクエリーを作成。

クエリーを ReciclerViewのアダプタにセットする。

```kotlin
        query = firestore.collection("restaurants")
            .orderBy("avgRating", Query.Direction.DESCENDING)
            .limit(LIMIT.toLong())

        // RecyclerView
        query?.let {
            adapter = object : RestaurantAdapter(it, this@MainFragment) {
            ...
            binding.recyclerRestaurants.adapter = adapter
        }
```

onStart / onStop でサーバの監視を開始・停止している

```kotlin
    override fun onStart() {
        super.onStart()
        ...

        // Start listening for Firestore updates
        adapter?.startListening()
    }

    override fun onStop() {
        super.onStop()
        adapter?.stopListening()
    }
```

#### レストラン情報をサーバに追加

コレクション restaurants に対して addを繰り返し行う。

```kotlin
        val restaurantsRef = firestore.collection("restaurants")
        repeat(10) {
            RestaurantUtil.getRandom(requireContext()).let {
                restaurantsRef.add(it)
            }
        }
    }
```


### ✔ FilterDialogFragment.kt

Filters オブジェクトを返すためのダイアログ。
firebaseとは無関係なのであまり読まなくて良い

### ✔ Filters.kt

firebaseに問い合わせを行うためのフィルタ条件を管理する。

### ✔ RatingDialogFragment.kt

口コミ情報を入力するためのダイアログ

### ✔ ★★★ RestaurantDetailFragment.kt

詳細画面。
firebaseへのアクセスがある。よく読む必要がある

fireStoreは "コレクション"として複数データを管理する。アクセスは `DocumentReference` を用いて行われる。

"コレクション"の中に "ドキュメント"がある。これが レストラン情報の Restaurant オブジェクトに対応する。

```kotlin
        restaurantRef = firestore.collection("restaurants").document(restaurantId)
```

口コミはレストラン情報の下に位置する "サブコレクション"として管理される。

```kotlin
        val ratingsQuery = restaurantRef
                .collection("ratings")
                .orderBy("timestamp", Query.Direction.DESCENDING)
                .limit(50)
```



サーバ上の変化はコールバックで受け取る。 監視の開始と停止は onStart/onStop で行われている

```kotlin
        // onStart
        restaurantRegistration = restaurantRef.addSnapshotListener(this)


        // onStop
        restaurantRegistration?.remove()
        restaurantRegistration = null
``````

fragmentは `EventListener<DocumentSnapshot>` を implement している。


```java
  public ListenerRegistration addSnapshotListener(
      @NonNull EventListener<DocumentSnapshot> listener) {
    return addSnapshotListener(MetadataChanges.EXCLUDE, listener);
  }
```



#### 口コミの投稿

口コミ投稿ダイアログから onRating が呼ばれる。その中で addRating が実行され、これが口コミを投稿する。

ここはトランザクションで複数処理を行なっている。

```kotlin
        return firestore.runTransaction { transaction ->
            ...
            transaction.set(restaurantRef, restaurant)
            transaction.set(ratingRef, rating)
            null
        }
```

### ✔ ★★★ adapter/FirestoreAdapter.kt

RestaurantAdapter, RatingAdapter の基底クラス

firebaseへのアクセスを行なっている

`Query`

- firestoreに対する問い合わせ条件 (query)
- 検索条件などもメソッドで定義する


`FirebaseFirestore`

#### startListening / stopListening

fragmentの onStart/onStop時に呼んでいる

`registration` オブジェクトを queryから作っている (queryに対して SnapshotListenerを登録)

```kotlin
            registration = query.addSnapshotListener(this)
```


#### addSnapshotListener の中身は?

`EventListener<QuerySnapshot>` を継承したクラスに `onEvent` メソッドを実装。 ここで `QuerySnapshot` オブジェクトを受け、更新の内容を把握することができる。

`QuerySnapshot` は0個以上の DocumentSnapshot を持っている。 onEventではそれを一つ一つ確認して対応する処理を行なっている。

```kotlin
        for (change in documentSnapshots!!.documentChanges) {
            ...
        }
```

- ADDED ... 新しいアイテムが追加された
- MODIFIED ... アイテムの内容が変更された (インデックス値が同じ場合は更新処理、異なる場合は削除+追加処理)
- REMOVED ... アイテムが削除された

ADDED, MODIFIED, REMOVED それぞれで snapshot オブジェクトを操作する。 snapshot は単純な ArrayList。

```kotlin
    private val snapshots = ArrayList<DocumentSnapshot>()
```

サーバ上のデータが変化 
  → firebaseが受信 
  → onEvent で notifyItemInserted / notifyItemChanged / notifyItemMoved / notifyItemRemoved を通知
  → ReciclerViewAdapter が受信, onBindViewHolderが呼ばれる
  → getSnapshot で DocumentSnapshot を Restaurant, Ratingに変換
  → 表示に情報を反映


### ✔ ★★ adapter/RatingAdapter.kt

口コミ情報のリスト表示

- `Query`
  - `FirestoreAdapter`で使用する。

- `DocumentSnapshot`
  - getSnapshot関数で snapshotオブジェクトを取得する (FirestoreAdapterのメソッド)
  - snapshotから `Restaurant`オブジェクトに変換してレイアウトに反映する。
  - アイテムタップ時は呼び出し元オブジェクトに対して onRestaurantSelected コールバックで snapshotを渡す。


### ✔ ★★ adapter/RestaurantAdapter.kt

レストランのリスト表示

- `Query`
  - `FirestoreAdapter`で使用する。

- `DocumentSnapshot`
  - getSnapshot関数で snapshotオブジェクトを取得する (FirestoreAdapterのメソッド)
  - snapshotから `Restaurant`オブジェクトに変換してレイアウトに反映する。
  - アイテムタップ時は呼び出し元オブジェクトに対して onRestaurantSelected コールバックで snapshotを渡す。



データバインディングを使う必要がないのはちょっと以外


### ✔ ★ model/Rating.kt

口コミ情報
firebase firestoreと連動するデータクラス

口コミ情報は投稿者と紐付けられている ... __FirebaseUser__

uid, displayName, emailが使われている

`@ServerTimestamp` アノテーション ... サーバ側で適切な値にセットしてもらうために付ける

### ✔ ★ model/Restaurant.kt

レストラン情報
firebase firestoreと連動するデータクラス

`@IgnoreExtraProperties` アノテーション ... データベース上にこのクラスで定義されていない情報があっても無視する

### ✔ util/AuthInitializer.kt

FirebaseAuthの初期化
オフラインでテストする際に有効

### ✔ util/FirestoreInitializer.kt

FirebaseFirestoreの初期化
オフラインでテストする際に有効

### ✔ util/RatingUtil.kt

口コミ情報の設定・表示の際に使うユーティリティ

### ✔ util/RestaurantUtil.kt

レストラン情報の設定・表示の際に使うユーティリティ

### ✔ viewmodel/MainActivityViewModel.kt

サインイン状態とフィルタ条件の管理

