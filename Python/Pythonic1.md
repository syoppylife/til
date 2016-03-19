# Pythonらしさ

 - Pythonでは「<span style="color:red">**らしい**</span>」書き方のことを「<span style="color:red">**Pythonic**</span>」と呼ぶ

 - ループ処理をする場合は **「for文」**ではなくイテレータ―**「iter()**」を使う
    - ※イテレータ―処理可能な値のことを**「イテラブルなオブジェクト」**と呼ぶ
    - ※Pythonのfor文は内部的にイテレータ―を利用している
 -  Pythonのfor文はイテレータ―の受け取りに特化しているためJavaの「for(int i=0; i<10; i++)」のような添え字（変数：i）は存在しない
 	- その場合「<span style="color:red">**enumerate()**</span>」を利用し、決して「for index in range(len(array)):」とはしない