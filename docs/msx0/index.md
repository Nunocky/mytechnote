# MSX0

## クロス開発

- [MSXのZ80で何か作る｜note](https://note.com/msx_z80_program)
- [MSX用クロス開発のすすめ（z88dk）](https://juangotoh.hatenablog.com/entry/2015/10/29/231107)
- [z88dk/z88dk: The development kit for over a hundred z80 family machines - c compiler, assembler, linker, libraries\.](https://github.com/z88dk/z88dk)
- [z88dkを使ったMSX向け開発環境の作り方](https://chikuwa-empire.com/computer/msx-devenv-001/)
- [msx-sdcc @ ウィキ](https://w.atwiki.jp/msx-sdcc/) sdccというコンパイラを使う

## 資料
- [ゲームプログラマの落書き帳：Z80 講座](https://xevi.web.fc2.com/z80/index.html)
- https://kizuna.5ch.net/test/read.cgi/i4004/1684919803/l50
- https://ryusendo.rdy.jp/wordpress/wp-content/uploads/2012/05/MSXRX.pdf

## C のコンパイル

dockerでイメージ z88dk/z88dk を使うのが簡単。

`test.c` を書く

```c
#include <stdio.h>

main() {
  printf("Hello World");
}
```

以下のようにしてコンパイルできる

```sh
zcc +msx -subtype=msxdos test.c -o test.com
```

作られた test.com を MSXDOSが動作する *.dskにコピーして動かすことができる。

> コンパイルオプションで '+msx'とつけることでMSX用になる。 -subtypeを指定しない場合、BASICからBLOADで読み込んで実行するバイナリになるようだ。-subtype=romというのもあって、ROMカセット形式のバイナリを作成できる。





## ディスクイメージにファイルをコピーする

macOSであれば [How to read \.dsk on MacOS \| MSX Resource Center](https://www.msx.org/forum/msx-talk/software/how-to-read-dsk-on-macos) に書かれていた方法が使える。


1. ディスクイメージの拡張子 dsk を imgに変更
2. finderでマウント
3. ファイルをコピー
4. アンマウント
5. 拡張子を戻す

[suzukiplan/msx\-disk\-manager\-cli: \[WIP\] MSX Disk Manager for CLI](https://github.com/suzukiplan/msx-disk-manager-cli) ではうまく行かなかった。MSX-DOSが起動しない。

## コンパイラについて

`msx.h`というヘッダがある
