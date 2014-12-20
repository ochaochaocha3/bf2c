bf2c
====

Ruby で書かれた、Brainfuck のソースをそれなりに読みやすい C 言語のソースに変換するプログラムです。標準入力から読み込み、標準出力に出力します。

実行例
------

例として、[Wikipedia に載っている Hello world](https://ja.wikipedia.org/wiki/Hello_world%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%A0%E3%81%AE%E4%B8%80%E8%A6%A7#Brainfuck)（`helloworld.bf`）を変換してみます。

```brainfuck
# Brainfuck で「Hello, world!」と表示する
# Wikipedia「Brainfuck」 (https://ja.wikipedia.org/wiki/Hello_world%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%A0%E3%81%AE%E4%B8%80%E8%A6%A7#Brainfuck) より

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
size_t p;

int main(void) {
  size_t i;

#define REPEAT(n) \
  for (i = 0; i < (n); i++)

  REPEAT(9) { a[p]++; }
  while (a[p] != 0) {
    p++;
    REPEAT(8) { a[p]++; }
    p++;
    REPEAT(11) { a[p]++; }
    p++;
    REPEAT(5) { a[p]++; }
    REPEAT(3) { p--; }
    a[p]--;
  }

  p++;
  putchar(a[p]);
  p++;
  REPEAT(2) { a[p]++; }
  putchar(a[p]);
  REPEAT(7) { a[p]++; }
  REPEAT(2) { putchar(a[p]); }
  REPEAT(3) { a[p]++; }
  putchar(a[p]);
  p++;
  a[p]--;
  putchar(a[p]);
  REPEAT(12) { a[p]--; }
  putchar(a[p]);
  p--;
  REPEAT(8) { a[p]++; }
  putchar(a[p]);
  REPEAT(8) { a[p]--; }
  putchar(a[p]);
  REPEAT(3) { a[p]++; }
  putchar(a[p]);
  REPEAT(6) { a[p]--; }
  putchar(a[p]);
  REPEAT(8) { a[p]--; }
  putchar(a[p]);
  p++;
  a[p]++;
  putchar(a[p]);

  return 0;
}
```

エラー
------

`[` と `]` の対応が取れていないとエラーが発生します。

* 開かれている `[` がないときに `]` が出現した：`excess ']'`
* `[` が閉じられないままファイルの末尾に達した：`lacks ']'`
