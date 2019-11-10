# envrc-helper

envrc-helperはdirenvを使う際に便利なコマンドです。
envrc-helperは`.envrc`と`.envrc.template`を管理します。

# What is .envrc.template?

`.envrc`は大体の場合、リモートレポジトリにプッシュ出来ないような秘密の値を持つため、`.gitignore`の中に書きます。
ただそうすると新しくそのレポジトリに参加した人が、開発に必要な環境変数をコードだけから知ることが出来ません。

それを防ぐために、下記のように`.envrc.template`に環境変数名だけを書いておけば、このファイルはリモートレポジトリにプッシュ可能です。

```.envrc.template
export SECRET_VARIABLE=
export OTHER_VARIABLE=
```

これは勝手にenvrc-helperが作るファイルなので、direnvそのものとは何も関係はありません。

# Installation

envrc-helperは純粋なbashシェルスクリプトで、特別なコマンドなどは使用していません。
（`direnv allow`は叩いています。）
なので、お好きなディレクトリに`src/envrc-helper`を置いてパスが通っていれば使えます。

# Usage

```
envrc-helper usage:

    envrc-helper COMMAND [ARGUMENTS ...]

    execute this command in a git repository root.

    COMMAND and ARGUMENTS:

        init: creates .envrc file and .envrc.template file and creates/updates .gitignore file.

        add NAME VALUE: adds NAME as an environment variable to .envrc and .envrc.template.

        remove NAME: removes NAME environment variable from .envrc and .envrc.template.

        create: creates .envrc file from .envrc.template.
```

## ~/.envrc.default

`~/.envrc.default`が存在する場合は、`.envrc`と`.envrc.template`を作る時にそれをコピーして作ります。


## Examples

```
$ mkdir new-project; cd new-project
$ git init
$ envrc-helper init
created .envrc
created .envrc.template
direnv: loading .envrc

$ cat .gitignore
.envrc

$ envrc-helper add SECRET_VARIABLE secret-value
direnv: loading .envrc
direnv: export +SECRET_VARIABLE

$ cat .envrc
export SECRET_VARIABLE=secret-value

# スペルミスったりしてもremoveで楽に消せます
$ envrc-helper add OTHER_VARIABL value
direnv: loading .envrc
direnv: export +SECRET_VARIABLE +OTHER_VARIABL

$ envrc-helper remove OTHER_VARIABL 
direnv: loading .envrc
direnv: export +SECRET_VARIABLE

$ envrc-helper add OTHER_VARIABLE value
direnv: loading .envrc
direnv: export +SECRET_VARIABLE +OTHER_VARIABLE

$ cat .envrc
export SECRET_VARIABLE=secret-value
export OTHER_VARIABLE=value

$ cat .envrc.template
export SECRET_VARIABLE=
export OTHER_VARIABLE=

$ git add .envrc.template; git commit -m "add .envrc.template"; git push origin branch
```