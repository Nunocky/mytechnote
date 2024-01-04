# Firebase

## どんな機能が?

- 認証 (Firebase Authentication)
- データベース (Realtime Database)
- データベース (Firestore)
- ストレージ (Storage)
- 機械学習 (ML)
- ホスティング
- Cloud Functions
- セキュリティルール
- App Check
- Extensions

頻繁に使うのは認証、データベース、ストレージくらいか。Cloud Functionsも気になるが有償だったはず。

Extensionsはベータ版なのでいまは見ない。ML,ホスティング、App Checkも今は必要ない。

セキュリティルールは必要になったら調べる。

## データベースの使い分けは?

https://firebase.google.com/docs/database/rtdb-vs-firestore?hl=ja

> Cloud Firestore は、Firebase のモバイルアプリ開発用の最新データベースです。直感的な新しいデータモデルで Realtime Database をさらに強化しています。Cloud Firestore は、Realtime Database よりも多彩で高速なクエリと高性能なスケーリングが特長です。
>
> Realtime Database は従来からある Firebase のデータベースです。リアルタイムのクライアント間同期が必要なモバイルアプリのための、効率的でレイテンシが低いソリューションです。

機能強化版の Cloud Firestoreを覚えてしまえばそれ以上勉強の必要がなさそう。難しすぎたら Realtime Databaseにする

