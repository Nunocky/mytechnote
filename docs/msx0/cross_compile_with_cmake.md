# cmake Template for cross compiling MSX C software on macOS

テンプレートを作った → https://github.com/Nunocky/msx_hello_world_c

- compiler : [z88dk](https://z88dk.org/site/)

## ビルド

```sh
cmake -B build -DCMAKE_TOOLCHAIN_FILE=../msx-toolchain.cmake .
```

