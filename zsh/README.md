# Комфорт в терминале

Сравните две картинки ниже:

![bash-vs-zsh](https://i.imgur.com/2edKk11.png)

Если вам было бы комфортнее работать в терминале слева, то можете дальше не читать.

В данном разделе мы посмотрим, как установить `zsh`,
как настроить его функциональность и как кастомизировать его внешний вид.

## Установка

### zsh

Официальную инструкцию можно найти [здесь](https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH), 
но если в двух словах, то:
* macOS: 
  ```shell script
  brew install zsh
  ```
* Ubuntu, Debian, Windows 10 WSL and alike:
  ```shell script
  sudo apt install zsh
  ```
* Arch, Monjaro:
  ```shell script
  pacman -S zsh
  ```

Сразу после установки лучше не стало (слева `bash`, справа `zsh`, оба на Ubuntu):

![after-install](https://i.imgur.com/Zc674oc.png)

С голым `zsh` можно работать, однако для настройки нам понадобится фреймворк `Oh My Zsh`.

### Oh My Zsh

[Тут](https://github.com/ohmyzsh/ohmyzsh) располагается полное описание, 
а для тех, кто в спешке (должен быть установлен `git`):
* curl:
  ```shell script
  sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
  ```
* wget:
  ```shell script
  sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
  ```

Теперь чуть по-красивее:

![after-oh-my-zsh](https://i.imgur.com/peWryj6.png)

Так выглядит автодополнение для `git`:

![git-autocomplete](https://i.imgur.com/l8BUuJk.png)

## Кастомизация

Основные настройки для `zsh` лежат по пути `~/.zshrc`:

![zshrc](https://i.imgur.com/mIOkVTW.png)

Мы рассмотрим несколько из них (по большей части, плагины и темы), всё остальное вы можете изучить сами.

### Plugins

Полный список можно найти [здесь](https://github.com/ohmyzsh/ohmyzsh/wiki/Plugins). 

Начнём с [подсветки синтаксиса](https://github.com/zsh-users/zsh-syntax-highlighting):
1. Клонируем репозиторий в папку `Oh My Zsh`:
   ```shell script
   git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
   ```
2. В файле настроек `~/.zshrc` добавляем `zsh-syntax-highlighting` в общий список плагинов:
   ![syntax-highlighting](https://i.imgur.com/73uJoUh.png)
   
При следующем запуске / в новой сессии / при выполнении `source ~/.zshrc` можно увидеть результат:
![after-syntax-highlighting](https://i.imgur.com/P8UGiGp.png)

Теперь черёд [автодополнений](https://github.com/zsh-users/zsh-autosuggestions), действия точно такие же:
1. Сначала клонируем репозиторий:
   ```shell script
   git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
   ```
2. Теперь регистрируем плагин `zsh-autosuggestions`:
   ![autosuggestions](https://i.imgur.com/DRu2POE.png)

Опять перезапускаем терминал / делаем новую сессию / `source ~/.zshrc` и получаем подсказки:
![after-autosuggestions](https://i.imgur.com/qeLbjoD.png)

Если цвет подсказок белый / сильно совпадает с цветом обычного текста, 
попробуйте поэкспериментировать с переменной `TERM`, например, вот так: `export TERM=xterm-256color`.

И ещё один плагин для быстрой навигации – [`z`](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/z):
1. Просто указываем его в списке:
   ![z](https://i.imgur.com/cETwJ27.png)

Теперь мы можем попасть в директорию, в которую хоть раз заходили при помощи `cd`, набрав лишь часть названия:
![after-z](https://i.imgur.com/fBrE289.png)

### Themes

Полный список тем можно найти [тут](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes).

Их много разных на любой цвет и вкус, но есть та, что выделяется сильнее всех остальных, – `Powerlevel10k`:
1. Сначала нужно [установить рекомендуемый шрифт](https://github.com/romkatv/powerlevel10k#meslo-nerd-font-patched-for-powerlevel10k)
2. Теперь клонируем репозиторий:
   ```shell script
   git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
   ```
3. Указываем тему `powerlevel10k/powerlevel10k` в `~/.zshrc`:
   ![powerlevel10k](https://i.imgur.com/L9enCUq.png)

Перезагружаемся и сразу попадаем в окно настройки. 
Внимательно читаем и отвечаем на все вопросы, настраиваем по вкусу и вуаля:
![after-powerlevel10k](https://i.imgur.com/ZdkmcrC.png)

### Terminal

Получить ещё больше возможностей настроить терминал можно, если установить новый терминал вместо дефолтного.
* macOS: однозначно [iTerm2](https://www.iterm2.com/) (именно он изображён на самой верхней картинке справа).
* Linux-подобные: можно попробовать [Guake](http://guake-project.org/), [Tilda](https://github.com/lanoxx/tilda), 
[Yakuake](https://github.com/KDE/yakuake).
