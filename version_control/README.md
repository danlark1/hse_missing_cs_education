# git и системы контроля версий

Системы контроля версий (или по-английски VCSs) &mdash; это утилиты для работы с исходным
кодом. Так как код меняется не везде и не всегда в репозитории, они стали одной
из основополагающих вещей современной разработки для её параллелизации.

На самом деле история разработки git была достаточно спонтанной. Линус Торвальдс,
создатель операционной системы Linux, устал мержить локально все патчи где-то в
конце 1990-х, что выливалось в то, что только у него была финальная часть кода,
а ему надоело чинить упавшие тесты, и он не мог быстро разослать всем текущую
версию кода. Тогда он стал использовать [BitKeeper](https://en.wikipedia.org/wiki/BitKeeper),
который позволял пушить коммиты и хранить последнюю версию кода где-то на
сервере. К сожалению, из-за небольшого скандала и плохого перформанса BitKeeper,
Линус после релиза одной из версии Linux ушёл на несколько месяцев в себя и
вышел с системой git, которая сейчас является стандартом в коллаборативной
разработке кода. По опыту, очень много студентов испытывают сложности с этой
программой скорее из-за того, что git унаследовал достаточно плохие имена
команд, которые надо запоминать, а большинство туториалов не рассказывают, что
находится "под капотом". Мне и многим моим знакомым стал понятен весь интерфейс
git только после того, как было рассказано про его сущности, как хранятся ветки,
что происходит при merge веток и т.д. Об этом сегодня и поговорим. Также разберём
наиболее популярные сценарии.

Стоит отметить, что существуют другие системы контроля версии, такие как
[svn](https://en.wikipedia.org/wiki/Apache_Subversion), которые больше похожи на
BitKeeper. svn отличается тем, что там ограниченный функционал веток, а
особенностью является простота и поддержка только одного финального state &mdash;
последняя версия. Такими идеями пользуется Google Piper &mdash; система контроля
версий на несколько миллиардов строк кода, git на очень больших масштабах
показывает себя плохого из-за длинных цепочек коммитов, сложного поиска и
огромного количества кода, который ещё и надо хранить. Для практически всего
open source всегда будет достаточно git, учитывая его сильную гибкость.

Системы контроля версий помогают писать код, где Вы знаете все изменения, кто
и когда их сделал.

Почему версии нужны? На самом деле, когда Вы работаете с кодом полезно взглянуть на
идеи за той или иной строчкой кода. Версии очень сильно помогают увеличить
продуктивность: можно работать над несколькими фичами независимо и только в
последний момент их объединить. git помогает разрешать конфликты изменений,
в общем, это помощник в коллаборативной работе с другими людьми. Не стоит
путать git и github, так как последнее это просто хранилище git репозиториев,
которое использует git по максимуму и задаёт стандарты open-source и
коллаборативной работы.

Современные системы контроля версий отвечают на следующие вопросы:

- Кто, когда и зачем написали этот модуль/кусок кода?
- Какой коммит стал виновником падений тестов?
- Кто в основном поддерживает репозиторий, к кому стоит обратиться?


В целом, ощущение git можно описать следующим комиксом, который мне очень сильно
полюбился [XKCD comic](https://xkcd.com/1597/). Как я уже писал выше, репутация
у git не самая хорошая, но поверьте, разработчики git стараются очень сильно
её исправить и у них потихоньку получается.

![xkcd 1597](https://imgs.xkcd.com/comics/git.png)

Несмотря на то, что у git плохой интерфейс (скажем спасибо Линусу), заложенные
идеи очень и очень красивы. Да, к сожалению, придётся запомнить много команд, но
как только Вы начнёте понимать, как они взаимодействуют, должно стать легче и
понятнее.

# Что git в себе хранит?

Во-первых, стоит понимать, когда Вы создаёте git репозиторий, то там будет
скрытая папка `.git`. Поговорим о том, что в ней находится.

## Snapshots

git моделирует историю изменений как набор файлов и папок, которые называются
в терминологии `blobs` и `trees` соответственно. `tree` &mdash; это таблица из имён в
`blobs` или `trees`. Snapshot (или снимок, если угодно) &mdash; это самое высокое
дерево, которое есть в репозитории, например:

```
<root> (tree)
|
+- foo (tree)
|  |
|  + bar.txt (blob, contents = "hello world")
|
+- baz.txt (blob, contents = "git is wonderful")
```

Корень дерева содержит два элемента: дерево "foo", которое содержит ещё blob
"bar.txt") и отдельный blob "baz.txt".

## Модель хранения версий

Как же хранить версии? Одна модель в одной цепочке. Так делает svn,
но в git из-за того, что очень хотел поддержать множество разработок даже над
одной версией и удобный merge, модель хранения версий другая.

Это просто ацикличный граф snapshots, о которых мы только что поговорили. У
каждого snapshot может быть несколько родителей, а потом они могут воедино
слиться в один.

В терминологии git эти snapshots &mdash; это коммиты, например:

```
o <-- o <-- o <-- o
            ^
             \
              --- o <-- o
```

После 3-го коммита создаётся отдельная ветка. Как я уже говорил, это может быть
отдельная логика, фича или что-то подобное. Потом они могут слиться воедино,
как владелец репозитория решит их помёржить.

<pre>
o <-- o <-- o <-- o <---- <strong>o</strong>
            ^            /
             \          v
              --- o <-- o
</pre>

Коммиты в git неизменяемые. Это может показаться странным. Как так? Ошибки
же случаются, хочется же что-то поменять, исправить опечатки и так далее. Тем
не менее, создастся новый snapshot, удалится старый, заменится на новый. Это
сделано, чтобы было легче обновлять репозиторий и знать, что случилось и как всё
изменилось.

## Ещё раз про модель данных

Она примерно такая:

```
// a file is a bunch of bytes
type blob = array<byte>

// a directory contains named files and directories
type tree = map<string, tree | blob>

// a commit has parents, metadata, and the top-level tree
type commit = struct {
    parent: array<commit>
    author: string
    message: string
    snapshot: tree
}
```

## Объекты

Объект это что угодно из `blob`, `tree`, `commit`.

```
type object = blob | tree | commit
```

В git все объекты хранятся по хэшу [SHA-1
hash](https://en.wikipedia.org/wiki/SHA-1), это удобно. Например, коммит
однозначно определяется хэшами предыдущих коммитов. Поэтому изменение где-то
посередине цепочки (мы рассмотрим, когда такое надо) изменит все последующие
хэши, что правильно, чтобы ни в какой момент не нарушать инвариант.

```
objects = map<string, object>

def store(object):
    id = sha1(object)
    objects[id] = object

def load(id):
    return objects[id]
```

Все объекты хранят только референсы на другие объекты и не хранят сами данные
(кроме `blob`, но они просто хранят ссылку на (возможно сжатый) файл).

Чтобы заглянуть под капот [snapshot](#snapshots), можно использовать команду
`git cat-file -p $SHA1`):

```
100644 blob 4448adbf7ecd394f42ae135bbeed9676e894af85    baz.txt
040000 tree c68d233a33c5c06e0340e4c224f0afca87c8ce87    foo
```

Мы видим, как указаны все родители объектов. Если мы хотим спуститься дальше,
то после `git cat-file -p 4448adb` мы получим следующее:

```
git is wonderful
```

## Референсы или ссылки

Не всегда удобно запоминать или каждый раз подсматривать SHA1 хэш (даже
несколько первых символов). git решил, что можно хранить ещё дополнительные
референсы на snapshots:

```
references = map<string, string>

def update_reference(name, id):
    references[name] = id

def read_reference(name):
    return references[name]

def load_reference(name_or_id):
    if name_or_id in references:
        return load(references[name_or_id])
    else:
        return load(name_or_id)
```

Например, git знает что такое референс на master, HEAD (то, где мы сейчас в
графе snapshot), все ветки разработки.
К ним можно применять операции, например HEAD\~n &mdash; отойти от HEAD на несколько
коммитов назад.

## Репозиторий

Наконец-то мы можем создать _репозиторий_. Заходим в папку и делаем `git init`,
пока всё просто) Обратите внимание на `.git` папку, там как раз есть все
объекты, о которых мы говорили выше. Они хранятся на диске и идут со всеми git
репозиториями. Все команды git так или иначе дальше будут манипулировать с графом
snapshot, добавляя и удаляя объекты, обновляя референсы.

Одна из больших трудностей &mdash; откат всех изменений, которые были сделаны. Если
Вы хотите откатиться на какую-то версию или вообще что-то сделать с графом
достаточно осмысленное то этому всегда найдётся способ (кстати, для удаления
всех коммитов до 5d83f9e, можно использовать `git checkout master; git reset
--hard 5d83f9e`).

# Staging фаза

Когда Вы что-то меняете в репозитории, оно не добавляется автоматически, оно
просто ничего не делает. Дальше Вы добавляете все эти
изменения, которые Вам нужны (например, не добавляя какие-нибудь ненужные debug
файлы) и можете переходить уже в стадию staging и потом в создание нового
snapshot.

# git CLI

Тут будет много команд, не очень много комментариев, много демонстраций на
лекции, смотрите запись, а также бесплатную книжку [Pro
Git](https://git-scm.com/book/en/v2).

## Начнём

`git init`, `git status`:

```console
$ mkdir myproject
$ cd myproject
$ git init
Initialized empty Git repository in myproject/.git/
$ git status
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

Никаких коммитов, давайте что-то нибудь сделаем:

```console
$ echo "hello, git" > hello.txt
$ git add hello.txt
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   hello.txt

$ git commit -m 'Initial commit'
[master (root-commit) 4515d17] Initial commit
 1 file changed, 1 insertion(+)
 create mode 100644 hello.txt
```

`git add` перевёл файлы в staging,  `git
commit` создал snapshot с сообщением "Initial commit". Без `-m`, открывается
Ваш дефолтный редактор для сообщения коммита.

Теперь можно визуализировать все изменения:

Команда `git log` помогает в этом. Чтобы было разноцветно и приятно с
картинками, советую `git log --all --graph --decorate`:

```console
$ git log --all --graph --decorate
* commit 4515d17a167bdef0a91ee7d50d75b12c9c2652aa (HEAD -> master)
  Author: Missing Semester <email>
  Date:   Wed Aug 19 19:17:55 2020 +0100

      Initial commit
```

Пока ещё не настоящий граф, так как состоит из одной вершины, давайте сделаем
что-то ещё:

```console
$ echo "another line" >> hello.txt
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   hello.txt

no changes added to commit (use "git add" and/or "git commit -a")
$ git add hello.txt
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   hello.txt

$ git commit -m 'Add a line'
[master 35f60a8] Add a line
 1 file changed, 1 insertion(+)
```

Теперь можно визуализировать и будет больше похоже на граф:

```
* commit 35f60a825be0106036dd2fbc7657598eb7b04c67 (HEAD -> master)
| Author: Missing Semester <email>
| Date:   Wed Aug 19:26:20 2020 +0100
|
|     Add a line
|
* commit 4515d17a167bdef0a91ee7d50d75b12c9c2652aa
  Author: Missing Semester <email>
  Date:   Wed Aug 19:17:55 2020 +0100

      Initial commit
```

Показан HEAD и соответствующая ветка master.

С помощью `git checkout` можно возвращаться на старые коммиты:

```console
$ git checkout 4515d17  # previous commit hash; yours will be different
Note: checking out '4515d17'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b <new-branch-name>

HEAD is now at 4515d17 Initial commit
$ cat hello.txt
hello, git
$ git checkout master
Previous HEAD position was 4515d17 Initial commit
Switched to branch 'master'
$ cat hello.txt
hello, git
another line
```

`git diff`, чтобы посмотреть изменения. Можно просто написать `git diff`, тогда
покажется staging vs HEAD. Можно коммит и blob/tree, можно два коммита и
blob/tree, если нет blob/tree, тогда покажутся все изменения:

```console
$ git diff 4515d17 hello.txt
diff --git c/hello.txt w/hello.txt
index 94bab17..f0013b2 100644
--- c/hello.txt
+++ w/hello.txt
@@ -1 +1,2 @@
 hello, git
 +another line
```

Итак, что надо точно знать:

- `git help <command>`: help по командам
- `git init`: создаём git репозиторий
- `git status`: что происходит
- `git add <blob/tree>`: добавляем blob/tree
- `git commit`: создаём новый коммит
    - Пишем [хорошее сообщение коммита](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)!
- `git log`: смотрим на то, что происходило
- `git log --all --graph --decorate`: красиво смотрим на то, что происходило
- `git diff <blob/tree>`: изменения blob/tree mdash; staging vs HEAD
- `git diff <revision> <blob/tree>`:  изменения blob/tree &mdash; revision vs HEAD
- `git checkout <revision>`: обновляем HEAD на revision

## Ветки и слияние веток

`git branch <branch name>` создаем
`git checkout -b <branch name>` сразу создаём и переходим

`git merge` сливаем ветки и резолвим конфликты:

- `git branch`: посмотреть все ветки
- `git branch <name>`: создаём ветку
- `git checkout -b <name>`: сразу создаём и переходим
    - тоже самое, что и `git branch <name>; git checkout <name>`
- `git merge <revision>`: сливаем ветку с текущей
- `git mergetool`: резолвим конфликты красиво
- `git rebase`: переназначаем часть графа на другой родитель, множество опций,
см. лекцию
  - `git rebase -i`: ну хоть как-то понятно перезначаем часть графа на другой
  родитель

## Храним на сервере

- `git remote`: все серверы, которые хранят нашу копию
- `git remote add <name> <url>`: добавляем то, где мы будем хранить
- `git push <remote> <local branch>:<remote branch>`: посылаем объекты на remote и обновляем remote ветку
- `git branch --set-upstream-to=<remote>/<remote branch>`: ставим соответствие между local и remote (github за Вас это делаем сам)
- `git fetch`: забираем изменения из remote
- `git pull`: забираем и сразу делаем merge, эквивалентно `git fetch; git merge`
- `git clone`: клонируем себе репозиторий

## Undo

- `git commit --amend`: обновляем последний коммит
- `git reset HEAD <blob/tree>`: убираем blob/tree из staging
- `git checkout -- <blob/tree>`: убираем все локальные изменения с blob/tree

# Advanced Git

- `git config`: git [кастомизируется](https://git-scm.com/docs/git-config)
- `git clone --depth=1`: скачиваем репозиторий и не храним всю историю, полезно для больших репозиториев
- `git add -p`: интерактивное добавление, например, почанково
- `git rebase -i`: делаем move в графе красиво
  - Обратите внимание! Иногда с Вас будут требовать сделать squash коммитов, например, Вы сделали очень много изменений мелких,
    а финально будет красиво иметь только один коммит, стоит сделать
    `git rebase -i d86f2765c..d6c5244c9` и поставить напротив всех коммитов,
    кроме первого/последнего метку squash
- `git blame`: кто что поменял в blob/tree
- `git stash`: убрать все локальные изменения и сохранить для будущего использования
  - `git stash pop`: вернуть все изменения, которые были сохранениы
- `git bisect`: бинарный поиск по коммитам, например, чтобы найти виновных в
  сломанном тесте
- `.gitignore`: [файл](https://git-scm.com/docs/gitignore) для блокировки
  каких-то расширений, которые не надо коммитить, например, сборка бинарей или
  библиотек

# Ещё много всего

- **GUIs**: есть много git [GUI клиентов](https://git-scm.com/downloads/guis).
Я лично не использую.
- **Shell**: В [Oh My Zsh](https://github.com/ohmyzsh/ohmyzsh) и
fish будет автодополнение по всем командам, референсам git, очень полезно.
- **Текстовые редакторы**: Для vim &mdash; [fugitive.vim](https://github.com/tpope/vim-fugitive),
в Sublime Text по умолчанию есть.
- **github**: git не github, это просто remote провайдер и удобным интерфейсом
для code review и merge изменений.
- **git providers**: github не единственный: как минимум есть
[GitLab](https://about.gitlab.com/) и
[BitBucket](https://bitbucket.org/).

# Ресурсы

- [Pro Git](https://git-scm.com/book/en/v2). **Просто прочитайте, умоляю Вас.**
- [Oh Shit, Git!?!](https://ohshitgit.com/). Забавный туториал
- [Git for Computer
Scientists](https://eagain.net/articles/git-for-computer-scientists/). Чуть
более подробно про модель данных
