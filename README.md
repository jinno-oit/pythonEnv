# 配布用python環境構築

### 構築するものの概要
- インストールフォルダ
  - `C:\\work\\py22`
    - Windows11などで，あらゆるフォルダがOneDriveと共有されている危険があるためCドライブ直下に配置する
- フォルダ構成
  - py22
    - code：プログラムは全てここに配置する．vscodeの設定もここへ配置する．
    - python-XX：embedded pythonをここに配置する．XXはバージョンである．
    - VSCode-win-x64-YY：vscode portableをここに配置する．YYはバージョンである．
    - console.bat：python-XXにパスが通ったcmdを開く．
    - vscode.bat：codeフォルダを作業フォルダとしたVSCodeを開く

### 1. embedded pythonをダウンロードする

- 以下のサイトから python: Windows embeddable package (64-bit) をダウンロードして展開する
  - https://www.python.org/downloads/windows/
- 以降，ダウンロードしたpythonバージョンはXX，展開したフォルダ名は`python-XX`とする
　- 展開したフォルダの絶対パスは`C:\\work\\py22\\python-XX`である

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
　- 展開したフォルダの絶対パスは`C:\\work\\py22\\vscode-win32-x64-YY`である

### 8. VSCodeをportableモードにする

- 展開したフォルダ`vscode-win32-x64-YY`の直下に`data`という空のフォルダを作成する

### 9. インストールフォルダ`py22`直下に`console.bat`と`vscode.bat`を配置する

- `console.bat`
```bash
@echo off
cd /D %~dp0

SET DP0=%~dp0
SET DP0=%DP0:~0,-1%

rem overriding windows user/local environment
SET LOCALENV_DIR=%DP0%\_local_env
SET TMP=%LOCALENV_DIR%\_tmp
SET TEMP=%LOCALENV_DIR%\_tmp
SET HOME=%LOCALENV_DIR%\userprofile
SET HOMEPATH=%LOCALENV_DIR%\userprofile
SET USERPROFILE=%LOCALENV_DIR%\userprofile
SET LOCALAPPDATA=%LOCALENV_DIR%\localappdata
SET APPDATA=%LOCALENV_DIR%\userroaming

SET PYTHON_PATH=%DP0%\python-XX #正しいバージョンに書き換える

rem overriding default python env vars in order to interfere with any system python installation
SET PYTHONHOME=
SET PYTHONPATH=
SET PYTHONEXECUTABLE=%PYTHON_PATH%\python.exe
SET PYTHONWEXECUTABLE=%PYTHON_PATH%\pythonw.exe

SET PYTHON_EXECUTABLE=%PYTHON_PATH%\python.exe
SET PYTHONW_EXECUTABLE=%PYTHON_PATH%\pythonw.exe
SET PYTHON_BIN_PATH=%PYTHON_EXECUTABLE%
SET PYTHON_LIB_PATH=%PYTHON_PATH%\Lib\site-packages
SET PATH=%PYTHON_PATH%;%PYTHON_PATH%\Scripts;%DP0%\CMake\Bin;%PATH%
SET DISTUTILS_USE_SDK=1

cd code
cmd
```
- `vscode.bat`
```bash
start "" .\VSCode-win32-x64-YY\Code.exe code
exit/b
```
### 10. VSCodeを起動し，拡張機能をインストールする

1. バッチファイル`vscode.bat`を利用してVSCodeを起動する
1. 例えば，以下のような拡張機能をインストールする
  - Python (これらも付いてくるJupyter, Pylance)
  - Japanese Lang Pack
  - Evillnspector

### 11. VSCodeの設定を調整する

- `C:\\work\\py22\\code\\.vscode`内に以下を配置する
  - バージョンやフォルダ名などは自身の環境に合わせること
- `settings.json`
```json
{
    "workbench.colorTheme": "Solarized Dark",
    "workbench.editorAssociations": {
        "*.ipynb": "jupyter.notebook.ipynb"
    },
    "python.analysis.extraPaths": [
        "C:\\work\\py22\\python-XX\\Lib\\site-packages",
        ".\\*"
    ],
    "python.defaultInterpreterPath": "C:\\work\\py22\\python-XX\\python.exe",
    "terminal.integrated.profiles.windows": {
        "Command Prompt": {
            "path": "${env:windir}\\system32\\cmd.exe",
            "args": [
                "/k",
                "C:\\work\\py22\\console.bat"
            ]
        }
    },
    "terminal.integrated.defaultProfile.windows": "Command Prompt",
    "update.enableWindowsBackgroundUpdates": false,
    "update.mode": "none",
    "update.showReleaseNotes": false,
    "extensions.autoUpdate": false,
    "extensions.autoCheckUpdates": false,
    "extensions.ignoreRecommendations": true,
    "python.autoUpdateLanguageServer": false,
    "editor.parameterHints": false,
    "editor.suggestSelection": "first",
    "vsinstellicode.modify.editor.suggestSelection": "automaticallyOverrodeDefaultValue",
    "window.zoomLevel": 1
}
```
- `launch.json`
```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Current File",
            "type": "python",
            "request": "launch",
            "program": "${file}",
            "console": "integratedTerminal",
            "env": {
                "PYTHONPATH": "${workspaceFolder};C:\\work\\py22\\python-XX"
            }
        }
    ]
}
```
- ユーザ設定としたければ，`C:\\work\\py22\\VSCode-win32-YY\\data\\user-data\\User`こちらにも配置する

### 12. 7zipを利用してインストールファイルを作成する

- 7zipをインストールする
- py22の中身を全選択してpy22.7zに固める（フォルダを選択して固めると展開時にパスが変わり，失敗する）
- 作業フォルダに以下を配置する
  - py22.7z
  - config.txt
```bash
;!@Install@!UTF-8!
Title="install py22"
Directory="C:\\work\\py22"
InstallPath="C:\\work\\py22"
BeginPrompt="install python+vscode to C:\\work\\py22"
;!@InstallEnd@!
```
  - 7zSD.sfx（バージョンによって動かなかった．調査中）
    - https://www.7-zip.org/sdk.html
　  - 中身から探す
- cmdで作業フォルダに移動し，以下のコマンドでexe化する
```cmd
copy /b 7zSD.sfx + config.txt + py22.7z py22_installer.exe
```
- `py22_installer.exe`というインストールファイルができる
