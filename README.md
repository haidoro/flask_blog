# Flaskで作るミニブログ

## FlaskのMTVフレームワーク

Flaskは Model, Template, View の3つからなるフレームワークです。

1. URL指定すると、定義しているURLに紐づけられた処理を**View**で行う。
2. データ（データベース）にアクセスするには**Model**を通す
3. 処理した結果を**Template**を通して表示する



## virtualenv環境作成

バーチャル環境にvirtualenvを使います。

virtualenvのインストール

```
sudo pip install virtualenv
```

flask_blogという環境を作るには、以下を実行

```
virtualenv flask_blog
```



環境に入るのは以下`cd flask_blog`で移動した後に次のコマンドを実行

ここはよく失敗するので注意。

要は`virtualenv`で作成した環境名のフォルダ内にbinフォルダが作成されます。binフォルダ内にactivateファイルがあるのでそれを実行すれば良いのです。

```python
source bin/activate
```

現在の仮想環境から出る。

```python
deactivate
```

## アプリケーションの初期設定

Flaskの導入

```
pip install Flask
```

## アプリケーション本体の作成

flask_blogフォルダ内にblogフォルダを作成します。

これで準備ができました。



## Hello Worldの表示

blogフォルダに`__init__.py`と`views.py`の2つのファイルを作成します。

通常`__init__.py`ファイルはモジュールあるいはパッケージの初期化に使います。

`__init__.py`

```
#__init__.py

from flask import Flask

app= Flask(__name__)

import blog.views
```



`views.py`

```
# views.py

from blog import app

@app.route('/')
def show_entries():
    return 'Hello World'
```

###  サーバーファイル

次にサーバファイルを作成します。**このファイルはflask_blogフォルダに入れます。**

`server.py`

```
# server.py

from blog import app

if __name__ == '__main__':
	app.run(debug=True)
```



### 表示

コマンドから次のコマンドで`http://127.0.0.1:5000/`などのようなURLが表示されますのでブラウザにURLを入れて表示します。

```
python server.py
```

コマンドを入力すると次の答えが返ってきます。

 \* Debug mode: on

 \* Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)

 \* Restarting with stat

 \* Debugger is active!

 \* Debugger PIN: 212-393-982

## config.py作成

config.pyを作成しておくと設定情報をここにまとめることができます。

まずはデバッグモードの記述をしてみます。

`config.py`をblogフォルダ内に作成します。

```
DEBUG = True
```



`server.py`にデバッグモードの記載が不要になるので次のように変更します。`run()`の引数を空にしています。

```
from blog import app

if __name__ == '__main__':
	app.run()
```

`__init__.py`にconfig.pyを使うように記述を追加

`app.config.from_object('blog.config')`を追加

```
from flask import Flask

app= Flask(__name__)
app.config.from_object('blog.config') #config.pyを使う時に使用

import blog.views
```

## Template

blogフォルダ内に`templates`を作成しておきます。さらにその中に`entries`フォルダを作成します。

* フォルダ名は<span style="color:red">templatesと複数形</span>になっているの注意

`entries`フォルダの中にindex.htmlファイルを作成します。

index.html

```
<!DOCTYPE html>
<html lang="ja">
<head>
	<meta charset="UTF-8">
	<title>Flask Brog</title>
</head>
<body>
<p><a href="/" class="navbar-brand">Flask Blog</a></p>
<p>投稿はありません。</p>	
</body>
</html>
```

views.py を次のように変更します。

```
from flask import request, redirect, url_for, render_template,flash, session
from blog import app

@app.route('/')
def show_entries():
    return render_template('entries/index.html')
```

## CSSはBootstrap

cssはcssフレームワークのBootstrapを活用します。

index.htmlのheadに次の内容を貼り付けます。

```
 <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
```

index.htmlのbodyの終了タグ`</body>`の直前に以下を貼り付けます。

```
    <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js" integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>
```

ブラウザに表示される内容が少しきれいになるはずです。

あとはBootstrapの使い方に従ってcssを充ていきます。

## ログインフォームの作成

### navバーの追加

bootstrapのナビゲーションバーを活用します。formなど不要なものは削除しておきます。

```
<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <a class="navbar-brand" href="#">Navbar</a>
  <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
  </button>

  <div class="collapse navbar-collapse" id="navbarSupportedContent">
    <ul class="navbar-nav mr-auto">
      <li class="nav-item active">
        <a class="nav-link" href="/">Home <span class="sr-only">(current)</span></a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="/login">ナビゲーション1</a>
      </li>
    </ul>
  </div>
</nav>
```

ここまでのindex.html

ナビゲーションの「ナビゲーション1」のリンク、つまりhref属性の値に記述する内容の紐付けはviews.pyで記述。この作業が紐付けとして重要になる。

```
<!DOCTYPE html>
<html lang="ja">
<head>
	<meta charset="UTF-8">
	<title>Flask Brog</title>
	<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
</head>
<body>
<div class="container">
	<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <a class="navbar-brand" href="#">タイトル</a>
  <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
  </button>

  <div class="collapse navbar-collapse" id="navbarSupportedContent">
    <ul class="navbar-nav mr-auto">
      <li class="nav-item active">
        <a class="nav-link" href="/">Home <span class="sr-only">(current)</span></a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="/login">ナビゲーション1</a>
      </li>
    </ul>
  </div>
</nav>
<p><a href="/" class="navbar-brand">Flask Blog</a></p>
<p>投稿はありません。</p>
</div>
    <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js" integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>	
</body>
</html>
```

### login.html作成

**templatesフォルダの直下**にlogin.htmlを作成します。

これが認証のページになります。

```
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <title>Flask Brog</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
</head>
<body>

<div class="container">
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
        <a class="navbar-brand" href="/">Flask Blog</a>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>

        <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="nav navbar-nav navbar-right">
                <li class="nav-item">
                    <a class="nav-link" href="/login">ログイン</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="/logout">ログアウト</a>
                </li>
            </ul>
        </div>
    </nav>


    <div class="blog-body">
        <form action="/login" method=post>
            <div class="form-group">
                <label for="InputTitle">ユーザ名</label>
                <input type="text" class="form-control" id="InputTitle" name=username>
            </div>

            <div class="form-group">
                <label for="InputPassword">パスワード</label>
                <input type="password" class="form-control" id="InputPassword" name=password>
            </div>
            <button type="submit" class="btn btn-primary">ログイン</button>
        </form>
    </div>
</div>
    <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js" integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>  
</body>
</html>
```

### アドレスの紐付け

アドレスの紐付けはviews.pyに記述します。

views.py

```
# blog/views.py
from flask import request, redirect, url_for, render_template,flash, session
from blog import app

@app.route('/')
def show_entries():
    return render_template('entries/index.html')

@app.route('/login',methods=['GET','POST'])
def login():
	if request.method == 'POST':
		if request.form['username']!=app.config['USERNAME']:
			print('ユーザー名が違います')
		elif request.form['password']!=app.config['PASSWORD']:
			print('パスワードが違います')
		else:
			return redirect('/')
	return render_template('login.html')

@app.route('/logout')
def logout():
	return rdirect('/')
```

### ユーザー名と暗証番号設定

ユーザー名と暗証番号はとりあえずconfig.pyに記述

config.py

```
# blog/config.py

DEBUG = True
USERNAME = 'tahara'
PASSWORD= '1234'
```

## templateの仕組み

templateは基本的にHTMLで作成します。

どのような時にも共通に使用する箇所を見つけます。

単純に考えてもHTMLの基本タグやheaderやfooterは共通になることが多いでしょう。そこでベースになるテンプレートを１つ作成します。

今回はlayout.htmlとしますが、名前は任意です。

考え方として、ベースになるテンプレートに入れ子状態で様々なテンプレートを作成していきます。それぞれ入れ子状態になるテンプレートはベーステンプレートを継承することになります。



入れ子になる個別のテンプレートはベーステンプレートを継承することになるので先頭に`{% extends "layout.html" %}`の記述をします。layout.htmlを継承したよ！となるわけです。

次にベーステンプレートに入れ子にする部分の印をつける必要があります。

その印が`{% block body %}{% endblock %}`の記述になります。

このコード部分に何らか入れ子のテンプレートが入るわけです。

再び入れ子になる個別のテンプレートにはここから、ここまでの内容がベーステンプレートに入りますという印をつけておきます。それが次のように`{% block body %}{% endblock %}`で囲むことになります。

```
{% block body %}
ここに入れ子のテンプレートの記述
{% endblock %}
```







### templateの効率化

複数の同じ内容の入ったテンプレートを作成することは非効率です。

従って、全てのテンプレートに共通の内容は1つにまとめる方法をとります。そのファイルをlayout.htmlとします。これはtemlatesフォルダの直下におきます。

layout.html

```
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <title>Flask Brog</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
</head>
<body>


<div class="container">
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
        <a class="navbar-brand" href="/">Flask Blog</a>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>

        <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="nav navbar-nav navbar-right">
                <li class="nav-item">
                    <a class="nav-link" href="/login">ログイン</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="/logout">ログアウト</a>
                </li>
            </ul>
        </div>
    </nav>

    <div class="blog-body">
        {% block body %}{% endblock %}
    </div>
</div>
    <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js" integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>  
</body>
</html>
```

index.htmlの編集

index.html

```
{% extends "layout.html" %}
{% block body %}
投稿がありません
{% endblock %}
```

login.htmlの編集

login.html

```
{% extends "layout.html" %}
{% block body %}
<form action="/login" method=post>
<div class="form-group">
    <label for="InputTitle">ユーザ名</label>
    <input type="text" class="form-control" id="InputTitle" name=username>
    </div>

    <div class="form-group">
    <label for="InputPassword">パスワード</label>
    <input type="password" class="form-control" id="InputPassword" name=password>
    </div>

    <button type="submit" class="btn btn-primary">ログイン</button>
    </form>
    {% endblock %}
```

ここまでの編集で表示内容が変わらないことを確認します。

## sessionの追加

session機能を追加します。

#### sessionの仕組み

1. サーバーからsession情報をブラウザに送る
2. ブラウザはcookieにsession情報を保管
3. リクエストの際にsession情報を持参してサーバーとクライアントを紐付ける

sessionを使用可能にするためにviews.pyの記述を変更します。

それと同時に、sessionにログイン済みの場合特定する値を保存する仕組みを作成します。

views.py

```
from flask import request, redirect, url_for, render_template, flash, session
from blog import app


@app.route('/')
def show_entries():
    if not session.get('logged_in'):
        return redirect('/login')
    return render_template('entries/index.html')


@app.route('/login', methods=['GET', 'POST'])
def login():
    error = None
    if request.method == 'POST':
        if request.form['username'] != app.config['USERNAME']:
            print('ユーザ名が異なります')
        elif request.form['password'] != app.config['PASSWORD']:
            print('パスワードが異なります')
        else:
            session['logged_in'] = True
            return redirect('/')
    return render_template('login.html')


@app.route('/logout')
def logout():
    session.pop('logged_in', None)
    return redirect('/')

```

セッションオブジェクトは辞書オブジェクトのように振る舞います。

従って次のように記述できます。

```
session['logged_in'] = True
```

ログイン成功後、sessionオブジェクトのキー 'logged_in' を True とすることでログイン状態を保存することができます。

ログインしてない状態の時は、次のようにログインフォームへリダイレクトします。

```
if not session.get('logged_in'):
        return redirect('/login')
```

ログアウトした時は`session['logged_in'] = True`の値を削除します。

```
session.pop('logged_in', None)
```

logout()では``session['logged_in']`の値を削除してindex.htmlへリダイレクトする内容です。

```
@app.route('/logout')
def logout():
    session.pop('logged_in', None)
    return redirect('/')
```

SECRET_KEY = 'secret key'をconfig.pyに追加します。

値はランダムな値を入れておきます。これは暗号化の際に使用されます。

config.py

```
DEBUG = True
SECRET_KEY = 'KufJdi86hdysk9oPlssyg'
USERNAME = 'tahara'
PASSWORD= '1234'
```





### login済みの場合のテンプレート変更

次にloginの有無でテンプレートのナビゲーションメニュー表示を変更する仕組みを作ります。

テンプレートには条件分岐の仕組みが用意されています。

{% %}内にif文と同様の記述ができます。

`session['logged_in'] `のことをここでは、`session.logged_in`とすることがポイント。

また、if文の終わりを表す{% endif %}を使うことに注目。

```
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <title>Flask Brog</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
</head>
<body>


<div class="container">
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
        <a class="navbar-brand" href="/">Flask Blog</a>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="nav navbar-nav navbar-right">
                {% if not session.logged_in %}
                <li class="nav-item">
                    <a class="nav-link" href="/login">ログイン</a>
                </li>
                {% else %}
                <li class="nav-item">
                    <a class="nav-link" href="/logout">ログアウト</a>
                </li>
                {% endif %}
            </ul>
        </div>
    </nav>

    <div class="blog-body">
        {% block body %}{% endblock %}
    </div>
</div>
    <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js" integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>  
</body>
</html>

```

ここでまたブラウザで動きを確認します。

## flash機能追加

flashはログイン成功や失敗など各アクション結果をメッセージにして表示する仕組みです。表示の仕方は違っても、用途としてjavascriptのalertのようなものです。

views.py

```
from flask import request, redirect, url_for, render_template, flash, session
from flask_blog import app


@app.route('/')
def show_entries():
    if not session.get('logged_in'):
        return redirect('/login')
    return render_template('entries/index.html')


@app.route('/login', methods=['GET', 'POST'])
def login():
    error = None
    if request.method == 'POST':
        if request.form['username'] != app.config['USERNAME']:
            flash('ユーザ名が異なります')
        elif request.form['password'] != app.config['PASSWORD']:
            flash('パスワードが異なります')
        else:
            session['logged_in'] = True
            flash('ログインしました')
            return redirect('/')
    return render_template('login.html')


@app.route('/logout')
def logout():
    session.pop('logged_in', None)
    flash('ログアウトしました')
    return redirect('/')

```

laout.htmlのナビゲーションの下に次の内容を追加します。

```
    {% for message in get_flashed_messages() %}
    <div class="alert alert-info" role="alert">
        {{ message }}
    </div>
    {% endfor %}
```

layout.html

```
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <title>Flask Brog</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
</head>
<body>


<div class="container">
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
        <a class="navbar-brand" href="/">Flask Blog</a>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="nav navbar-nav navbar-right">
                {% if not session.logged_in %}
                <li class="nav-item">
                    <a class="nav-link" href="/login">ログイン</a>
                </li>
                {% else %}
                <li class="nav-item">
                    <a class="nav-link" href="/logout">ログアウト</a>
                </li>
                {% endif %}
            </ul>
        </div>
    </nav>
    {% for message in get_flashed_messages() %}
    <div class="alert alert-info" role="alert">
        {{ message }}
    </div>
    {% endfor %}
    <div class="blog-body">
    {% block body %}{% endblock %}
    </div>
</div>
    <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js" integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>  
</body>
</html>

```

ここでブラウザで適切にflashが動作するか動作確認をします。

## url_for 自動リンク作成

URLリンクは通常 href="/login"などリンクのパスを記述していますが、url_forメソッドを使うと自動でリンクを設定します。

url_for()の引数にviews.pyに定義しているメソッド名を指定することで直接リンクに変更します。

views.py

```
from flask import request, redirect, url_for, render_template, flash, session
from flask_blog import app


@app.route('/')
def show_entries():
    if not session.get('logged_in'):
        return redirect(url_for('login'))
    return render_template('entries/index.html')


@app.route('/login', methods=['GET', 'POST'])
def login():
    error = None
    if request.method == 'POST':
        if request.form['username'] != app.config['USERNAME']:
            flash('ユーザ名が異なります')
        elif request.form['password'] != app.config['PASSWORD']:
            flash('パスワードが異なります')
        else:
            session['logged_in'] = True
            flash('ログインしました')
            return redirect(url_for('show_entries'))
    return render_template('login.html')


@app.route('/logout')
def logout():
    session.pop('logged_in', None)
    flash('ログアウトしました')
    return redirect(url_for('show_entries'))

```

layout.htmlの中のhrefの値を変更します。　

layout.html

```
 <ul class="nav navbar-nav navbar-right">
                {% if not session.logged_in %}
                <li class="nav-item">
                    <a class="nav-link" href="{{url_for('login')}}">ログイン</a>
                </li>
                {% else %}
                <li class="nav-item">
                    <a class="nav-link" href="{{url_for('logout')}}">ログアウト</a>
                </li>
                {% endif %}
            </ul>
```

login.htmlのactionの値を変更します。

login.html

```
<form action="{{url_for('login')}}" method=post>
```

ここで再度ブラウザで確認します。

## データベース

Flaskで簡単にデータベースを設定することができるSQLAlchemyというライブラリを使います。

### SQLAlckemyのインストール

```
pip install Flask-SQLAlchemy
```

### SQLAlckemyの初期設定

SQLiteを使う設定はconfig.pyと`__init__.py`に以下の内容を記述することでデータベースが使えるようになります。

config.py

```
SQLALCHEMY_DATABASE_URI = 'sqlite:///blog.db'
SQLALCHEMY_TRACK_MODIFICATIONS = True
DEBUG = True
SECRET_KEY = 'KufJdi86hdysk9oPlssyg'
USERNAME = 'tahara'
PASSWORD= '1234'
```

`__init__.py`

```
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

app= Flask(__name__)
app.config.from_object('blog.config')

db = SQLAlchemy(app)

import blog.views
```

## Modelの作成

blogフォルダの中にmodelsフォルダを作成して、entries.pyファイルを作成します。

entries.py

```
from flask_blog import db
from datetime import datetime

class Entry(db.Model):
    __tablename__ = 'entries'
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(50), unique=True)
    text = db.Column(db.Text)
    created_at = db.Column(db.DateTime)

    def __init__(self, title=None, text=None):
        self.title = title
        self.text = text
        self.created_at = datetime.utcnow()

    def __repr__(self):
        return '<Entry id:{} title:{} text:{}>'.format(self.id, self.title, self.text)

```

entries.pyの内容を反映させるコマンドを入力

```
python
>>>from blog import db
>>>db.create_all()
```

blog.dbがblogフォルダにできていることを確認します。

## スクリプトを使う

flaskにはスクリプトを管理実行するFlask-Scriptというライブラリがあります。

#### Flask-Scriptライブラリ導入

次のコマンドでインストールします。

```
pip install Flask-Script
```

blogフォルダの中にscriptsフォルダを作成してdb.pyを作成します。

```
from flask_script import Command
from blog import db


class InitDB(Command):
    "create database"

    def run(self):
        db.create_all()
```

次にmanage.pyを作成します。これはアプリケーションのルートフォルダに作成します。（server.pyがある場所）

```
from flask_script import Manager
from blog import app

from blog.scripts.db import InitDB


if __name__ == "__main__":
    manager = Manager(app)
    manager.add_command('init_db', InitDB())
    manager.run()

```



## CRUD

CRUDとはCreate,Read,Update,Deleteのことです。

アプリケーションを作成する時にCRUDを満たすと最低限の機能が満たされます。

### views.pyの分割

viewsフォルダを作成して、entries.pyとshow_entries.pyに分けて記述します。

viewsフォルダには空の`__init__.py`ファイルを置いておきます。

views.py

```
from flask import request, redirect, url_for, render_template, flash, session
from blog import app


@app.route('/login', methods=['GET', 'POST'])
def login():
    error = None
    if request.method == 'POST':
        if request.form['username'] != app.config['USERNAME']:
            flash('ユーザ名が異なります')
        elif request.form['password'] != app.config['PASSWORD']:
            flash('パスワードが異なります')
        else:
            session['logged_in'] = True
            flash('ログインしました')
            return redirect(url_for('show_entries'))
    return render_template('login.html')


@app.route('/logout')
def logout():
    session.pop('logged_in', None)
    flash('ログアウトしました')
    return redirect(url_for('show_entries'))

```



entries.py

```
from flask import request, redirect, url_for, render_template, flash, session
from blog import app
from blog.models.entries import Entry

@app.route('/')
def show_entries():
    if not session.get('logged_in'):
        return redirect(url_for('login'))
    return render_template('entries/index.html')

@app.route('/entries/new', methods=['GET'])
def new_entry():
    if not session.get('logged_in'):
        return redirect(url_for('login'))
    return render_template('entries/new.html')

@app.route('/entries', methods=['POST'])
def add_entry():
    if not session.get('logged_in'):
        return redirect(url_for('login'))
    entry = Entry(
        title=request.form['title'],
        text=request.form['text']
    )
    db.session.add(entry)
    db.session.commit()
    flash('新しく記事が作成されました')
    return redirect(url_for('show_entries'))
```

blogフォルダ直下の`__init__.py`ファイルの　viewsのインポートの記述を変更しておきます。

```
# blog/__init__.py

from flask import Flask
from flask_sqlalchemy import SQLAlchemy

app= Flask(__name__)
app.config.from_object('blog.config')

db = SQLAlchemy(app)

from blog.views import views, entries
```

## ブログ投稿フォームの作成

ブログ投稿フォームを作成

### ナビゲーションバーに新規投稿メニューを追加

```
<ul class="nav navbar-nav navbar-right">
                {% if not session.logged_in %}
                <li class="nav-item">
                    <a class="nav-link" href="{{url_for('login')}}">ログイン</a>
                </li>
                {% else %}
                <li class="nav-item">
                    <a class="nav-link" href="{{url_for('new_entry')}}">新規投稿</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{{url_for('logout')}}">ログアウト</a>
                </li>
                {% endif %}
            </ul>
```

### ブログ投稿フォーム

templates/entriesフォルダの中にnew.htmlファイルを作成

new.html

```
{% extends "layout.html" %}
{% block body %}
<form action="{{ url_for('add_entry') }}" method=post class=add-entry>
<div class="form-group">
    <label for="InputTitle">タイトル</label>
    <input type="text" class="form-control" id="InputTitle" name=title>
    </div>

    <div class="form-group">
    <label for="InputText">本文</label>
    <textarea class="form-control" id="InputText" name=text rows="3"></textarea>
    </div>
    <button type="submit" class="btn btn-primary">作成</button>
    </form>
    {% endblock %}
```

##ブログ投稿機能

### add_entryビュー作成

views/entries.pyにadd_entryビューを追加します。

```
from flask import request, redirect, url_for, render_template, flash, session
from blog import app
from blog.models.entries import Entry

@app.route('/')
def show_entries():
    if not session.get('logged_in'):
        return redirect(url_for('login'))
    return render_template('entries/index.html')

@app.route('/entries/new', methods=['GET'])
def new_entry():
    if not session.get('logged_in'):
        return redirect(url_for('login'))
    return render_template('entries/new.html')

@app.route('/entries', methods=['POST'])
def add_entry():
    if not session.get('logged_in'):
        return redirect(url_for('login'))
    entry = Entry(
        title=request.form['title'],
        text=request.form['text']
    )
    db.session.add(entry)
    db.session.commit()
    flash('新しく記事が作成されました')
    return redirect(url_for('show_entries'))

```

## Read機能

show_entriesビューで記事をデータベースから取得します。

views/entries.pyにshow_entriesビューを追加します。

views/entries.py

```
from flask import request, redirect, url_for, render_template, flash, session
from blog import app
from blog import db
from blog.models.entries import Entry

@app.route('/')
def show_entries():
    if not session.get('logged_in'):
        return redirect(url_for('login'))
    entries = Entry.query.order_by(Entry.id.desc()).all()
    return render_template('entries/index.html', entries=entries)


@app.route('/entries', methods=['POST'])
def add_entry():
    if not session.get('logged_in'):
        return redirect(url_for('login'))
    entry = Entry(
            title=request.form['title'],
            text=request.form['text']
            )
    db.session.add(entry)
    db.session.commit()
    flash('新しく記事が作成されました')
    return redirect(url_for('show_entries'))


@app.route('/entries/new', methods=['GET'])
def new_entry():
    if not session.get('logged_in'):
        return redirect(url_for('login'))
    return render_template('entries/new.html')


```

### ループを使ってテンプレートを作成

index.html

```
{% extends "layout.html" %}
{% block body %}
<ul class="list-group list-group-flush">

{% for entry in entries %}
<div class="card">
    <div class="card-body">
        <h5 class="card-title">{{ entry.title }}</h5>
    </div>
</div>
{% else %}
投稿がありません
{% endfor %}
</ul>
{% endblock %}
```



テーブル作成のコマンド

```
python manage.py init_db
```



### 続きを読むを追加

index.html

```
{% extends "layout.html" %}
{% block body %}
<ul class="list-group list-group-flush">

{% for entry in entries %}
<div class="card">
    <div class="card-body">
        <h5 class="card-title">{{ entry.title }}</h5>
        <a href="{{ url_for('show_entry', id=entry.id) }}" class="card-link">続きを読む</a>
        </div>
        </div>
        {% else %}
        投稿がありません
        {% endfor %}
        </ul>
        {% endblock %}

```



entries.py

```
from flask import request, redirect, url_for, render_template, flash, session
from blog import app
from blog import db
from blog.models.entries import Entry

@app.route('/')
def show_entries():
    if not session.get('logged_in'):
        return redirect(url_for('login'))
    entries = Entry.query.order_by(Entry.id.desc()).all()
    return render_template('entries/index.html', entries=entries)


@app.route('/entries', methods=['POST'])
def add_entry():
    if not session.get('logged_in'):
        return redirect(url_for('login'))
    entry = Entry(
            title=request.form['title'],
            text=request.form['text']
            )
    db.session.add(entry)
    db.session.commit()
    flash('新しく記事が作成されました')
    return redirect(url_for('show_entries'))


@app.route('/entries/new', methods=['GET'])
def new_entry():
    if not session.get('logged_in'):
        return redirect(url_for('login'))
    return render_template('entries/new.html')

@app.route('/entries/<int:id>', methods=['GET'])
def show_entry(id):
    if not session.get('logged_in'):
        return redirect(url_for('login'))
    entry = Entry.query.get(id)
    return render_template('entries/show.html', entry=entry)

```



show.html

```
{% extends "layout.html" %}
{% block body %}

<h2>{{ entry.title }}</h2>
<br>

{{ entry.text }}

<br><br>
投稿日時 {{ entry.created_at }}

{% endblock %}

```

