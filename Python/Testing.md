# Pythonでのテスト方法

## doctest
docstringに動作確認時のコードを記述し、検証することができる。

- 実行コマンド
```
$ python -m doctest ファイル名
```

- 例１：doctestが成功
```python
# -*- coding: utf-8 -*-


def get_ok():
    """
    文字列'OK'を返す

    >>> get_ok()
    'OK'
    """
    return 'OK'


if __name__ == '__main__':
    get_ok()

```
- テスト結果

```エラーが存在しないので何も表示されない```

- 例２：doctestに失敗
```python
# -*- coding: utf-8 -*-


def get_ok():
    """
    文字列'OK'を返す

    >>> get_ok()
    'OK'
    """
    return 'NG'


if __name__ == '__main__':
    get_ok()

```


- テスト結果

```
Failure
**********************************************************************
File "C:/doctest_sample_ng.py", line 8, in get_ok
Failed example:
    get_ok()
Expected:
    'OK'
Got:
    'NG'

```

### docstring
クラスや関数などの説明を　三重クォートで括ったコメントで記述する。#コメントと違いPythonインタプリタに解釈される。
docstringの情報は`help()`で表示できる。

## unittest
docstringはあくまでドキュメント用なので、複雑な処理を書いてしまうと読みにくくなってしまう。
その場合はPython標準ライブラリのunittestを使用する。

- 例：unittestの書き方

```python
import unittest
from python_yousei_dokuhon import add


class Test(unittest.TestCase):
    def test_add(self):
        value = add.add(1,1)
        self.assertEqual(2, value)
```
※`unittest.TestCase`を継承する

- 実行コマンド
```
$ python -m unittest ファイル名
```
※`$ python -m unittest discover`でディレクトリ配下の全てのテストモジュールのテストを実行


- 例１：unittestが成功
```python
def add(m, n):
    """mとnを加算して返す"""
    return m + n
```

- テスト結果
```
.
----------------------------------------------------------------------
Ran 1 test in 0.000s

OK

```


- 例２：unittestが失敗
```python
def add(m, n):
    """mとnを加算して返す"""
    return m - n
```

- テスト結果
```
Traceback (most recent call last):
  File "C:\test.py", line 8, in test_add
    self.assertEqual(2, value)
AssertionError: 2 != 0

----------------------------------------------------------------------
Ran 1 test in 0.002s

FAILED (failures=1)
```