# envrc-helper

envrc-helper is a usefull command for user of direnv.
envrc-helper makes it easy that creates/updates `.envrc` and `.envrc.template`.

# What is .envrc.template?

`.envrc` file is usually in a `.gitignore` because it has secret values that we shouldn't push to remote repositories.
You can push `.envrc.template` instead because it has only environment variable names, like below.

```.envrc.template
export SECRET_VARIABLE=
export OTHER_VARIABLE=
```

Developers that are new to a repository can easily grasp environment variables that development of the repository needs by `.envrc.template`.

# Installation

Actually envrc-helper is a simple bash shellscript and depends on no special commands or packages excluding direnv.

Copy `src/envrc-helper` into a directory in $PATH, and you'll be able to use it.

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

If `~/.envrc.default` exists, `envrc-helper` copies it when creating `.envrc.template`.
Create it if you want to let `.envrc` have default common environment variables.


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

# oops, misspelling...
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