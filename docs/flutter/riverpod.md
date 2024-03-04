# Riverpod

## プロバイダについて
それぞれのプロバイダの使い分けは、保持する値の型と、値の変更頻度によって異なります。

- Providerは、単一の値を保持する場合に使用します。
- Future Providerは、Future<T>型の値を保持する場合に使用します。
- Stream Providerは、Stream<T>型の値を保持する場合に使用します。
- Notifier Providerは、値を変更する必要がある場合や、値を複数のウィジェットで共有する場合に使用します。
- AsyncNotifier Providerは、値を変更する必要がある場合や、値を複数のウィジェットで共有する場合、かつ値の取得に時間がかかる場合に使用します。


### Provider

Providerは、最も基本的なプロバイダです。単一の値を保持します。

### Future Provider

Future Providerは、Future<T>型の値を保持します。Futureが完了すると、値がウィジェットに配信されます。

### Stream Provider

Stream Providerは、Stream<T>型の値を保持します。Streamがエミットされると、値がウィジェットに配信されます。

### Notifier Provider

Notifier Providerは、値を変更できるプロバイダです。値を変更すると、変更がウィジェットに反映されます。

NotifierProviderを使うときには、対応する Notifierを作る必要がある。

```dart
final counterNotifier = NotifierProvider((ref) => CounterNotifier());

class CounterNotifier extends StateNotifier<int> {
  CounterNotifier() : super(0);

  void increment() {
    state = state + 1;
  }
}
```

### AsyncNotofier Provider

AsyncNotifier Providerは、値を変更できるプロバイダであり、Future<T>型の値を保持します。値を変更すると、変更がウィジェットに反映されます。



## family?

Riverpodで出てくるfamilyは、複数のプロバイダをまとめて管理するための機能です。familyを使用すると、複数のプロバイダを一度に登録したり、複数のプロバイダから値を取得したりすることができます。

TODO あとで調べる

## keepAlive?

Flutter Riverpodのkeepaliveは、Widgetが画面から非表示になっても、Providerが保持されるようにするための機能です。

通常、Widgetが画面から非表示になると、そのWidgetで保持されているProviderも破棄されます。これは、メモリを効率的に使用するために必要なことです。しかし、一部のProviderは、画面から非表示になっても保持しておきたい場合があります。例えば、ユーザーのログイン状態や、現在再生中の音楽の再生位置などです。

keepaliveを使用すると、これらのProviderを画面から非表示になっても保持することができます。これにより、Widgetが再表示された際に、Providerからデータを取得し直す必要がなくなります。

TODO あとで調べる


## Providerの選び方

https://zenn.dev/flutteruniv_dev/articles/riverpod_generator_in_action

> どの Provider を使ったら良いのかはよく迷うポイントかと思います。ここからは自分の勝手な解釈も交えつつ、どういうポイントで選定するかについて軽く触れます。当たり前っちゃ当たり前な事書いてる可能性は大です(笑)
>
>私の中では以下のような基準で選定しています。

| ユースケース                                                | プロバイダ                     |
| ----------------------------------------------------------- | ------------------------------ |
| Stateless なクラスだけ注入したい                            | Provider                       |
| 複数の Provider の値を加工して注入したい                    | Provider                       |
| 非同期な状態値だけ注入したい                                | FutureProvider, StreamProvider |
| 状態値の他にメソッドも注入したい + 状態値の初期値が同期的   | NotifierProvider               |
| 状態値の他にメソッドも注入したい + 状態値の初期値が非同期的 | AsyncNotifierProvider          |

> NotifierProvider でも実は AsyncValue を状態値として返すことが可能です。しかし AsyncNotifierProvider を使えば、自動的に状態値が AsyncValue として返されるので、非同期な状態値を返すのであれば、素直に AsyncNotifierProvider を使うのが良いかと思います。

## レガシーな Notifier

https://zenn.dev/shintykt/articles/020858a88d1536

- StateProvider -> Provier
- StateNotifier -> Notifier, AsyncNotofier

にそれぞれ置き換える。

