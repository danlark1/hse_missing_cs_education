# Домашнее задание

Это репозиторий, взятый из достаточно популярного репозитория по упражнению git &mdash; [git-katas](https://github.com/eficode-academy/git-katas). Не волнуйтесь, решений там нет. А сдать вам их придётся :)

## Задание

Будет дано 33 задачи на git, вам надо будет их всех
проделать, сдать каст вашего терминала, как Вы делали то или иное задание.

Для выполнения вам надо будет использовать Linux/MacOS, [WSL на Windows](https://www.windowscentral.com/install-windows-subsystem-linux-windows-10) или
Virtual Box на той же Windows (установить можно по видео инструкции как [здесь](https://www.youtube.com/watch?v=hhrf7KbR-lM)).

Для сдачи задач вам также надо будет установить утилиту [`asciinema`](https://asciinema.org/docs/installation).
Эта утилита open source, она не отправляет ваши данные никому.

При сдаче задач мы требуем от вас полной записи вашего терминала.
Если вы меняете файлы с помощью UI редактора,
делайте это в этот раз с помощью vim.

Чтобы записать ваш терминал, делайте

```console
$ asciinema rec PROBLEM.cast
```

где PROBLEM дословное название папки, в которой находится задание.

Если задача требует написания рассуждения, откройте отдельный файл при записи и
напишите текстом это рассуждение вместе с пунктом задачи, для которого
предназначено это рассуждение.

В итоге надо будет сдать .zip архив всех записей всех задач, которые вы решили.
Сдавать по ссылке TODO. Дедлайн 24 октября 2021 23:59:59 MSK

Чтобы просмотреть ваш каст, можно делать

```console
$ asciinema play PROBLEM.cast
```

С дополнительным флагом `-s` для скорости. Например, `-s 3` в 3 раза ускоряет
каст.

Просим не делать долгих решений. Мы не хотим смотреть решения по 30 минут.
Перезапишите каст, если есть проблемы со временем.

Учтите, что на лекции было много про философию и устройство git, практика
закрепляется здесь.

## Оценивание

Каждая задача оценивается в 0.32 балла. Можно получить больше 10 баллов. После
основ вы получите примерно 6.4 баллов, после стандартных проблем 8 баллов, а
после продвинутых технологий все 10.

В каждой задаче есть нумерованные пункты. Штрафы за невыполнение пунктов
пропорциональны, то есть если есть y пунктов, вы сделали x, то одна задача
оценивается как `x/y * 0.32`.

Внимательно читайте каждый пункт. Большинство ошибок будут из-за плохого
прочтения пунктов. Все задачи на английском языке.

Проверка ручная. Если вы хотите получить дополнительных бонусов, можете
записаться на проверку своих одногруппников. Мы добавим 0.5-1 в итог за
помощь нам. Напишите лектору, если вы заинтересованы.

## Setup

1. [configure-git](configure-git/README.md) - Сконфигурируйте git.

## Основы

1. [basic-commits](basic-commits/README.md) - Научитесь коммитить.
2. [basic-staging](basic-staging/README.md) - Поиграйте со `stage` состоянием.
3. [basic-branching](basic-branching/README.md) - Ознакомьтесь с ветками.
4. [ff-merge](ff-merge/README.md) - Помёржьте различные ветки.
5. [3-way-merge](3-way-merge/README.md) - А теперь посложнее merge.
6. [merge-conflict](merge-conflict/README.md) - А теперь в ветках есть конфликты.
7. [merge-mergesort](merge-mergesort/README.md) - Конфликт с вашим кодом.
8. [rebase-branch](rebase-branch/README.md) - Играемся с rebase.
9. [basic-revert](basic-revert/README.md) - Откатываем изменения.
10. [reset](reset/README.md) - Изучаем опасную команду reset.
11. [basic-cleaning](basic-cleaning/README.md) - Очищаем генерируемые файлы.
12. [amend](amend/README.md) - Склеиваем коммит к предыдущему.
13. [reorder-the-history](reorder-the-history/README.md) - Меняем порядок коммитов.
14. [squashing](squashing/README.md) - Склеиваем много коммитов в один.
15. [advanced-rebase-interactive](advanced-rebase-interactive/README.md) - Играемся с интерактивным rebase.
16. [basic-stashing](basic-stashing/README.md) - Написали кода, надо обновиться, а коммитить не хочется. На помощь приходит stash.
17. [ignore](ignore/README.md) - Пишем `.gitignore` файлы. Удаляем с помощью `git rm`.
18. [submodules](submodules/README.md) - Интегрируем сабмодули.
19. [git-tag](git-tag//README.md) - Играемся с тегами коммитов.

## Решаем стандартные проблемы

1. [commit-on-wrong-branch](commit-on-wrong-branch/README.md) - Если мы случайно поместили неопубликованные коммиты в неправильную ветку, как нам эффективно _переместить_ их в другую ветку, прежде чем работать над ней?
2. [commit-on-wrong-branch-2](commit-on-wrong-branch-2/README.md) - Посложнее предыдущего.
3. [reverted-merge](reverted-merge/README.md) - Откатываем merge, а потом накатываем с фиксами.
4. [save-my-commit](save-my-commit/README.md) - Удалили коммит? Узнайте как его вернуть.
5. [detached-head](detached-head/README.md) - git жалуется, что вы "You are in 'detached HEAD' state". Что делать?

## Продвинутые фичи

1. [git-attributes](git-attributes/README.md) - .gitattributes файл позволяет настроить то, как вы будете работать с концами строк или сравнивать бинарные файлы.
2. [Bad-commit](bad-commit/README.md) - Настройте бисекцию `git bisect`, чтобы найти плохой коммит.
3. [bisect](bisect/README.md) - Посложнее сценарий бинарного поиска `git bisect`.
4. [pre-push](pre-push/README.md) - Настраиваем git хуки.
5. [Investigation](investigation/README.md) - Смотрим, что под капотом git репозитория.
6. [Objects](objects/README.md) - И ещё.
7. [merge-driver](merge-driver/README.md) - Устанавливаем свой драйвер для слияний.
8. [rebase-exec](rebase-exec/README.md) - Гоняем тесты на все ваши коммиты с помощью `git rebase --exec`.

## Cheatsheet

Полезные команды:

```sh
# Initializing an empty git repository.
git init            # Initialize an empty git repository under current directory.

# Cloning a repository
git clone https://github.com/danlark1/hse_missing_cs_education.git      # Clone this repository to your current working directory

# Git (user and repo level) configurations
git config --local user.name "Repo-level Username"          # For setting a local git repo level user name.
git config --local user.email "Repo-level.Email@Example.com" # For setting a local git repo level user email.
                                                            # --global -> User level git config stored in <user-home>/.gitconfig for e.g. ~/.gitconfig
                                                            # --local -> repo level config stored in repo's main dir under .git/config


# See local changes
git status                  # Show the working tree status
git diff                    # Show changes current working directory (not yet staged)
git diff --cached           # Show changes currently staged for commit

# Add files to staging (before a commit)
git add myfile.txt          # Add myfile.txt to stage
git add .                   # Add entire working directory to stage

# Make a commit
git commit                              # Make a new commit with the changes in your staging area. This will open an editor for a commit message.
git commit -m "I love documentation"    # Make a new commit with a commit message from the command line
git commit -a                           # Make a new commit and automatically "add" changes from all known files
git commit -am "I still do!"            # A combination of the above
git commit --amend                      # Re-do the commit message of the previous commit (don't do this after pushing!)
                                        #   We _never_ change "public history"
git reset <file>                        # Unstage a staged file leaving in working directory without losing any changes.
git reset --soft [commit_hash]          # resets the current branch to <commit>. Does not touch the staging area or the working tree at all.
                                        # --hard mode would discard all changes.

# Configuring a different editor
## Avoid Vim but stay in terminal:
- `git config --global core.editor nano`

## For Windows:
- Use Notepad:
`git config --global core.editor notepad`

- or for instance Notepad++:
`git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"`


# See history
git log             # Show commit logs
git log --oneline   # Formats commits to a single line (shorthand for --pretty=oneline  --abbrev-commit )
git log --graph     # Show a graph commits and branches
git log --pretty=fuller     # To see commit log details with author and committer details, if any different.
git log --follow <file>     # List the history of a file beyond renames
git log branch2..branch1    # Show commits reachable from branch1 but not from branch2

# Deferring
git stash                               # Stash (store temporarily) changes in working branch and enable checkingout a new branch
git stash list                          # List stored stashes.
git stash apply <stash>                 # Apply given <stash>, or if none given the latest from stash list.


# Working with Branches
git branch my-branch       # Create a new branch called my-branch
git checkout my-branch     # Checkout ("Switch" to work on) my-branch
git checkout -b my-branch  # Create a new branch called my-branch AND switch to it
git branch -d my-branch    # Delete branch my-branch that has been merged with master
git branch -D my-branch    # Forcefully delete a branch my-branch that hasn't been merged to master

# Merging
git merge master         # Merge the master branch into your currently checked out branch.
git rebase master        # Rebase current branch on top of master branch

# Working with Remotes
git remote              # Show your current remotes
git remote -v           # Show your current remotes and their URLs
git push                # Publish your commits to the upstream master of your currently checked out branch
git push -u origin my-branch  # Push newly created branch to remote repo setting up to track remote branch from origin.
                              # No need to specify remote branch name, for e.g., when doing a 'git pull' on that branch.
git pull                # Pull changes from the remote to your currently checked out branch

# Re/moving files under version control
git rm <path/to/the/file>                 # remove file and stage the change to be committed.
git mv <source/file> <destination/file>   # move/rename file and stage the change to be committed.

# Aliases - it's possible to make aliases of frequently used commands
#   This is often done to make a command shorter, or to add default flags

# Adding a shorthand "co" for "checkout"
git config --global alias.co "checkout"
# Usage:
git co      # Does a "git checkout"

## Logging
git log --graph --oneline --all # Show a nice graph of the previous commits
## Adding an alias called "lol" (log oneline..) that shows the above
git config --global alias.lol "log --graph --oneline --all"
## Using the alias
git lol     # Does a "git log --graph --oneline --all"
```

### Cleanup

Вы можете удалить все ненужные артифакты, `exercise` директории с помощью `git clean` команды:

```sh
git clean -ffdX
```

Оно не удалит ваши решения, не переживайте.
