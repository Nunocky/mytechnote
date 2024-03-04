# Cで Hello World

dockerでイメージ z88dk/z88dk を使うのが簡単。

## `test.c` を書く

```c
#include <stdio.h>

main() {
  printf("Hello World");
}
```

## コンパイル
以下のようにしてコンパイルできる

```sh
zcc +msx -subtype=msxdos test.c -o test.com
```

作られた test.com を MSXDOSが動作する *.dskにコピーして動かすことができる。

> コンパイルオプションで '+msx'とつけることでMSX用になる。 -subtypeを指定しない場合、BASICからBLOADで読み込んで実行するバイナリになるようだ。-subtype=romというのもあって、ROMカセット形式のバイナリを作成できる。

