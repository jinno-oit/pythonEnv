# 配布用python環境構築

### 1. embedded pythonをダウンロードする

- 以下のサイトから python: Windows embeddable package (64-bit) をダウンロードして展開する
  - https://www.python.org/downloads/windows/
- 以降，ダウンロードしたpythonバージョンはXX，展開したフォルダ名は`python-XX`とする
- また，インストールフォルダは`C:\\work\\py22`とする
　- 展開したフォルダのパスは`C:\\work\\py22\\python-XX`である

### 2. 展開したフォルダ（`python-XX`）直下の`pythonXX._path`を編集する

- 以下をコメントインする
```python
import site
```

### 3. `python-XX`直下に`current.pth`と`get-pip.py`を配置する

- `current.path`
```python
import sys;sys.path.append('')
```
- `get-pip.py`
  - 以下からダウンロードする（リンクが死んだ場合，github get-pipなどで検索する）
    - https://github.com/pypa/get-pip/blob/main/public/get-pip.py

### 4. cmdで`python-XX`へ移動し，`get-pip.py`を実行する

- 以下のコマンドで実行する
```cmd
python.exe get-pip.py
```

### 5. tkinterをインストールする

参考：https://qiita.com/Neilus/items/17b44d737d5acd1982fb
1. ダウンロードしたembedded pythonと同じバージョンのpythonをインストールする
1. `tkinter系`のフォルダやファイルを`python-XX`直下にコピーする
  - インストールしたpythonフォルダのLibの中にある以下をコピーする
  　- tkinterフォルダ
  　- tclフォルダ
  - インストールしたpythonフォルダのDLLsの中にある以下をコピーする
  　- _tkinter.pyd
  　- tcl86t.dll（バージョンによって数字などは違う）
  　- tk86t.dll（バージョンによって数字などは違う）
  - おまけ：`pythonXX._path`にパスを追加するなら，DLLsフォルダに配置しても良い

### 6. モジュールをインストールする

- 以下のコマンドで配布時に入れておくモジュールをインストールする
```cmd
python -m pip install [module]
```
  - numpy
  - matplotlib
  - opencv-python
  - opencv-contrib-python
  - mediapipe
  - msvc-runtime
  - etc.

### 7. VSCodeをダウンロードする

- 以下のサイトから vscode: Windows .zip 64bit をダウンロードして展開する
  - https://code.visualstudio.com/download#
- 以降，ダウンロードしたVSCodeバージョンはYY，展開したフォルダ名は`vscode-win32-x64-YY`とする
- また，インストールフォルダは`C:\\work\\py22`とする
　- 展開したフォルダのパスは`C:\\work\\py22\\vscode-win32-x64-YY`である

### 8. VSCodeをportableモードにする

- 展開したフォルダ`vscode-win32-x64-YY`の直下に`data`という空のフォルダを作成する

### 9. VSCodeを起動し，拡張機能をインストールする

- 例えば，以下のような拡張機能をインストールする
　- Python (これらも付いてくるJupyter, Pylance)
　- Japanese Lang Pack
　- Evillnspector

### 10. VSCodeの設定を調整する

- 


