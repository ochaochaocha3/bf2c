bf2c
====

Ruby で書かれた、Brainfuck のソースをそれなりに読みやすい C 言語のソースに変換するプログラムです。標準入力から読み込み、標準出力に出力します。

実行例
------

例として、[Wikipedia に載っている Hello world](https://ja.wikipedia.org/wiki/Hello_world%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%A0%E3%81%AE%E4%B8%80%E8%A6%A7#Brainfuck)（`helloworld.bf`）を変換してみます。

```brainfuck
# Hello world

+++++++++[>++++++++>+++++++++++>+++++<<<-]>.>++.+++++++..+++.>-.
------------.<++++++++.--------.+++.------.--------.>+.
```

以下を実行します。

```bash
cd /path/to/bf2c
./bf2c < helloworld.bf > helloworld.c
```

実行すると、以下の内容の `helloworld.c` が得られます。

```c
#include <stdio.h>

#define ARRAY_SIZE 32768

unsigned char a[ARRAY_SIZE];
unsigned char* ptr = a;

int main(void) {
  size_t i;

#define REPEAT(n) \
  for (i = 0; i < (n); i++)

  *ptr += 9;

  while (*ptr != 0) {
    ptr++;
    *ptr += 8;
    ptr++;
    *ptr += 11;
    ptr++;
    *ptr += 5;
    ptr -= 3;
    (*ptr)--;
  }

  ptr++;
  putchar(*ptr);
  ptr++;
  *ptr += 2;
  putchar(*ptr);
  *ptr += 7;
  REPEAT(2) { putchar(*ptr); }
  *ptr += 3;
  putchar(*ptr);
  ptr++;
  (*ptr)--;
  putchar(*ptr);
  *ptr -= 12;
  putchar(*ptr);
  ptr--;
  *ptr += 8;
  putchar(*ptr);
  *ptr -= 8;
  putchar(*ptr);
  *ptr += 3;
  putchar(*ptr);
  *ptr -= 6;
  putchar(*ptr);
  *ptr -= 8;
  putchar(*ptr);
  ptr++;
  (*ptr)++;
  putchar(*ptr);

  return 0;
}
```

エラー
------

`[` と `]` の対応が取れていないとエラーが発生します。

* 開かれている `[` がないときに `]` が出現した：`excess ']'`
* `[` が閉じられないままファイルの末尾に達した：`lacks ']'`
