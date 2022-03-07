# WindowsのPython環境を作る

Windows上でTensorflow GPUを使用できる環境を、管理者権限で実行するバッチファイルを用いて実装する。

大まかな方法としては、パッケージマネージャのChocolateyを使用してminiconda等をインストールし、minicondaをPythonの実行環境にして、必要な環境変数を設定する。

## 作業手順

### install.batを管理者権限で実行して必要な内容をインストールと環境変数を設定

以下の内容が実行されます。

- 環境変数をWindowsに設定
- chocolateryをインストール
- chocolateryで、`choco.config`ファイルに記載されている内容をインストール
- tensorという仮想環境を作成して`requirements.txt`の内容をインストール

https://blog.kintarou.com/2021/06/25/post-1591/

```batch
rem 実行は管理者権限で実行してください。
.\install.bat
```

#### このスクリプトを実行することで設定されるシステム環境変数

| 環境変数名                | 内容                                                         |
| ------------------------- | ------------------------------------------------------------ |
| MINICONDA_PATH            | MINICONDAのインストール先と、各種実行ファイルの設置先パス    |
| PATH                      | MINICONDA_PATHを追記する。                                   |
| ChocolateyInstall         | Chocolateryインストール時に設定される。                      |

#### このスクリプトを実行することで設定されるユーザ環境変数

| 環境変数名               | 内容                                    |
| ------------------------ | --------------------------------------- |
| ChocolateyLastPathUpdate | Chocolateryインストール時に設定される。 |
| ChocolateyToolsLocation  | Chocolateryインストール時に設定される。 |

## 実行時のエラーのヒント

実行時にエラーが出る場合は以下の内容に気をつける

- 実行ファイルの改行コードがCRLFではない。
- `python`とタイプしてもMicrosoftStoreが起動する場合は、`$PATH`の一番上にPythonのパスが来ていないのが原因。https://stackoverflow.com/questions/58754860/cmd-opens-window-store-when-i-type-python
- `install.bat`を実行すると`---- 'PATH' environment variable exceeds 1024 characters ----`と表示される場合、`PATH`を設定する時にコマンドプロンプトでの文字数制限の上限、1024文字以上になったため`PATH`が設定されない。