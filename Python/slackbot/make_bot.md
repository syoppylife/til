# Pythonでslackbotを作る方法

##1. 普通にSlackAPIを使ってメッセージをPOSTする

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
import urllib.request
import urllib.parse

def main():
    
    url    = 'https://slack.com/api/chat.postMessage'
    params = {'token' : 'your slac token', 
              'channel' : '#general', 
              'text' : '@anyone: 今日もお仕事お疲れ様☆', 
              'username' : '石原さとみ', 
              'icon_url' : 'https://...(googleの画像検索の結果など', 
              'link_names' : '1'}
    params = urllib.parse.urlencode(params)
    params = params.encode('utf-8')
    # リクエスト生成
    req = urllib.request.Request(url, params)
    # ヘッダ追加
    req.add_header('Content-Type', 'application/x-www-form-urlencoded')
    # POST
    res = urllib.request.urlopen(req)
    # レスポンス取得
    body = res.read()

    print (body.decode('utf-8'))


if __name__ == '__main__':
    main()
```

- token = slack上で取得したトークン
- channel = 投稿したい先のチャンネル名
- text = 投稿したいメッセージ
- username = 投稿した際に表示される名前
- icon_url = 投稿した際に表示されるアイコン画像（サイズは48/48じゃなくてもきれいに表示さるが正方形じゃないと崩れる）
- link_names = 1にするとメンションやチャンネル名がリンクになる


##2. Slackのインテグレーション「Bot」を使った場合

**Slack Botをインストール**

```python
$ sudo pip3 install slackbot
```

**Slack Botの起動用ファイルと設定ファイルを作成**

```
$ mkdir slackbot
$ cd slackbot
$ touch slackbot_settings.py
$ touch run.py
```

```
$ vi slackbot_settings.py
```

```python
# -*- coding: utf-8 -*-
 
API_TOKEN = "your Integration Bot's Token "
 
default_reply = ""
```

```
$ vi run.py
```


```python
# -*- coding: utf-8 -*-
 
from slackbot.bot import Bot


def main():
    bot = Bot()
    bot.run()
 
 
if __name__ == "__main__":
    main()

```

**プラグインを作成**

```
$ mkdir plugins
$ touch plugins/__init__.py
$ touch plugins/mention.py
```

```
$ vi plugins/mention.py
```

```python
# -*- coding: utf-8 -*-

from slackbot.bot import respond_to
import random


@respond_to('.*')
def cheer(message):
    msg_tuple = ("好きかも・・・", "いまどこ・・・？", "電話してほしいな・・・", "今週末楽しみだね☆", "気安く話しかけないで！", "そんなに無理しなくていいんだよ・・？")
    
    message.reply(random.choice(msg_tuple))

```

`@respond_to` に反応させたい文字列を設定。
上記は複数行設定可能。

`message.reply` でこのBotにメンションを送ったユーザーにメンションでランダムな文言を返す。

※Slack上のBot作成画面で好きな芸能人やアイドルの画像や名前を設定して、メンションしてみよう！



**プラグインをの読み込み**

```
$ vi run.py
```


```python
# -*- coding: utf-8 -*-
 
from slackbot.bot import Bot
from plugins import mention

def main():
    bot = Bot()
    bot.run()
 
 
if __name__ == "__main__":
    main()

```

**いざ起動！**

```
$ python3 run.py
```

任意の部屋にBOTを招待して適当にメンションをする！