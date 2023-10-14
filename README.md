# 「【Django】デプロイのための事前準備」 ITC youtube

2023/10/14

## settings.py の書き換え

プロジェクトフォルダ内に settings フォルダを作成

settings.py を settings フォルダに移動し、lacals.py に名前変更

settings フォルダ内に __init__.py を作成  

```python
try:
    from .base import *
except:
    from .locals import *
    ```

locals.pyの編集
    BASE_DIR = の記述の右端に.parentを1つ追加  
    さらに変数 PARENT_DIR も記述  

'''python
BASE_DIR = Path(__file__).resolve().parent.parent.parent
PARENT_DIR = Path(__file__).resolve().parent.parent.parent.parent
```

## 公開したくない情報、パスワードを隠すためにフォルダを追加

プロジェクトフォルダと同じ階層に auth と site という２つのフォルダを作成

site の中に logs と public というフォルダを作成

auth フォルダの中に .env というファイルを作り、環境変数として、隠したい値を保存する

隠したい値  

- secret_key
- name_db
- pswd_db
- user_db

シークレットキーを隠す手順

- シークレットキーの再作成  
  仮想環境フォルダで、
  ``` python ```  
  ``` >>> from django.core.management.utils import get_random_secret_key ```  
  ``` >>> get_random_secret_key() ```  

- .envフォルダに格納  
  secret_key = *****  ..''は外す  

- locals.py で読み込むために  
  dotenvのインストール  
  ``` pip install python-dotenv ```
  必要なモジュールのインポート  
  - ``` from dotenv import load_dotenv ```  
  - ``` import os ```  
  env_pathの記述  
  ``` env_path = PARENT_DIR / "auth/.env" ```  
  環境変数の読み込みの記述  
  ``` load_dotenv(env_path) ```  
  SECRET_KEYの書き換え  
  ``` SECRET_KEY = os.environ.get("secret_key") ```  

migrate を実行し、変わりないか確認  
``` python manage.py makemigrations ```  
``` python manage.py migrate ```  
local_server を起動し、/adminも確認  
``` python manage.py runserver ```  
