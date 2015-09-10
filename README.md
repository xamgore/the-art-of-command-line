# Искусство командной строки

- [Описание](#Описание)
- [Основы](#Основы)
- [Ежедневное использование](#Ежедневное-использование)
- [Процессинг файлов и информации](#Процессинг-файлов-и-информации)
- [Системный дебаггинг](#Системный-дебаггинг)
- [В одну строчку](#В-одну-строчку)
- [Сложно, но полезно](#Сложно-но-полезно)
- [MacOS X only](#Macos-x-only)
- [Больше информации по теме](#Больше-информации-по-теме)
- [Дисклеймер](#Дисклеймер)


Продвинутому использованию командной строки зачастую не уделяют достаточного внимания. О терминале говорят, как о чем-то мистическом. На самом же деле, это умение очевидно (и не очевидно) увеличивает Вашу продуктивность в работе. Данный документ является подборкой заметок и советов, которые я нашел для себя полезными, работая с командной строкой в Linux. Некоторые из их них – простые и очевидные, но некоторые - довольно сложные. И предназначены для решения конкретных задач. Это небольшая публикация, но если Вы уже знаете обо всем, что тут написано, и можете вспомнить, как это все использовать – вы много знаете!

Многое из того, что тут написано, [изначально](http://www.quora.com/What-are-some-lesser-known-but-useful-Unix-commands)
[появилось](http://www.quora.com/What-are-the-most-useful-Swiss-army-knife-one-liners-on-Unix)
на [Quora](http://www.quora.com/What-are-some-time-saving-tips-that-every-Linux-user-should-know),
начав идею там, похоже, что стоит развить ее на Github, где обитают люди, которые талантливее меня и могут предлагать улучшения данной подборки. Если Вы заметили ошибки (во всех вариантах перевода), пожалуйста оставьте тикет или киньте пулл-реквест (заранее изучив описание и посмотрев на уже созданнные тикеты и пулл-реквесты).

## Описание

Основное:

- Данная публикация предназначена как для новичков, так и для опытных людей. Цели: *объемность* (собрать все важные аспекты использования командной строки), *практичность* (давать конкретные примеры для самых частых юзкейсов) и *краткость* (не стоит углубляться в неочевидные вещи, о которых можно почитать в другом месте).
- Этот документ написан для пользователей Linux, с единственным исключеним – секцией "[MacOS only](#macos-only)". Все остальное подходит и может быть установлено под все UNIX/MacOS системы (и даже Cygwin).
- Фокусируемся на интерактивном Баше, но многие вещи также могут быть использованы с другими шеллами; и в общем применимы к Баш-скриптингу.
- Эта инструкция включает в себя стандартные Unix команды и те, для которых нужно устанавливать сторонние пакеты. Они настолько полезны, что стоят того, чтобы их установили.

Заметки:

- Для того, чтобы руководство оставалось одностраничным, вся информация вставлена прямо сюда. Вы достаточно умные для того, чтобы самостоятельно изучить вопрос более детально в другом месте. Используйте `apt-get`/`yum`/`dnf`/`pacman`/`pip`/`brew` (в зависимости от вашей системы управления пакетами) для установки новых программ.

- На [Explainshell](http://explainshell.com/) можно найти простое и подробное объяснение того, что такое команды, флаги, пайпы и т.д.

## Основы

- Выучите основы Баша. Просто возьмите и напечатайте `man bash` в терминале и хотя бы просмотрите его; он довольно просто читается и он не очень большой. Другие шеллы тоже могут быть хороши, но Баш – мощная программа, и Баш всегда под рукой (использование *исключительно* zsh, fish и т.д., которые наверняка круто выглядят на Вашем ноуте во многом Вас ограничивает, например Вы не сможете использовать возможности этих шеллов на уже существующем сервере).

- Выучите как использовать хотя бы один консольный редактор текста. Идеально Vim (`vi`), ведь у него нет конкурентов, когда вам нужно быстренько что-то подправить (даже если Вы постоянно сидите на Emacs/какой-нибудь тяжелой IDE или на каком-нибудь модном хипстерском редакторе)

- Знайте как читать документацию через `man` (для любознательных – `man man`; `man` по углам документа в скобках добавляет номер, например 1 – для обычных команд, 5 – для файлов, конвенций, 8 – для административных команд). Ищите мануалы через `apropos`, и помните, что некоторые команды – не бинарники, а встроенные команды Баша, и помощь по ним можно получить через `help` и `help -d`.

- Узнайте о том, как перенаправлять ввод и вывод через `>` и `<` и пайпы `|`. Помните, что `>` – переписывает выходной файл, а `>>` добавляет к нему. Узнайте побольше про stdout и stderr.

- Узнайте побольше про раскрытие file glob элементов `*` (а также `?` и `{`...`}`), кавычки, а также разницу между двойными `"` и одинарными `'` кавычками. (Больше о расширении переменных читайте ниже.)

- Будьте знакомы с работой с процессами в Bash: `&`, **ctrl-z**, **ctrl-c**, `jobs`, `fg`, `bg`, `kill`, и т.д.

- Знайте `ssh` и основы беспарольной аутентификации через `ssh-agent`, `ssh-add`, и т.д.

- Основы работы с файлами: `ls` и `ls -l` (в частности, узнайте, что значит каждый столбец в  `ls -l`), `less`, `head`, `tail` и `tail -f` (или даже лучше – `less +F`), `ln` и `ln -s` (узнайте разницу между символьными ссылками и жёсткими ссылками, и почему жёсткие ссылки лучше), `chown`, `chmod`, `du` (для быстрой сводки по использованию диска: `du -hk *`). Для менеджмента файловой системы, `df`, `mount`, `fdisk`, `mkfs`, `lsblk`.

- Основы работы с сетью: `ip` или `ifconfig`, `dig`.

- Хорошо знайте регулярные выражения и разные флаги к `grep`/`egrep`. Такие флаги как `-i`, `-o`, `-A`, и `-B` стоит знать.

- Обучитесь использованию системами управления пакетами `apt-get`, `yum`, `dnf` или `pacman` (в зависимости от дистрибутива). Знайте как искать и устанавливать пакеты и обязательно имейте установленым `pip` для установки командных утилит, написаных на Python (некоторые из тех, что вы найдёте ниже, легче всего установить через `pip`).

## Ежедневное использование

- Используйте таб в Баше для автокомплита аргументов к командам и **ctrl-r** для поиска по истории командной строки.

- Используйте **ctrl-w** в Баше для того, чтобы удалить последнее слово в команде; **ctrl-u** для того, чтобы удалить команду полностью. Используйте **alt-b** и **alt-f** для того, чтобы бегать между словами команды, **ctrl-k** для того, чтобы прыгнуть к концу строки, **ctrl-l** для того, чтобы очистить экран. Гляньте на `man readline` чтобы узнать о всех шорткатах Баша. Их много! Например, **alt-.** бежит по предыдущим аргументам команды, а **alt-*** расширяет глоб.??

- Если Вам нравятся шорткаты vim, сделайте `set -o vi`.

- Для того, чтобы посмотреть историю, введите `history`. Также существует множество аббревиатур, например `!$` – последний аргумент, `!!` – последняя команда, хотя эти аббревиатуры часто заменяются шорткатами **ctrl-r** и **alt-.**.

- Для того, чтобы прыгнуть к последней рабочей директории, используйте `cd -`

- Если Вы написали команду наполовину и вдруг передумали, нажмите **alt-#** для того, чтобы добавить `#` к началу, и отправьте команду, как комментарий. Потом вы сможете вернуться к ней через историю.

- Не забывайте использовать `xargs` (или `parallel`). Это очень мощная штука. Обратите внимание, что Вы можете контролировать количество команд на каждую строку, а также параллельность. Если Вы не уверены, что делаете что-то правильно, начните с `xargs echo`. Еще `-I{}` – полезная штука. Примеры:
```bash
      find . -name '*.py' | xargs grep some_function
      cat hosts | xargs -I{} ssh root@{} hostname
```

- `pstree -p` – полезный тип вывода дерева процессов.

- Используйте `pgrep` или `pkill` для того, чтобы находить/слать сигналы к процессам по имени (`-f` помогает).

- Знайте разные сигналы, которые можно слать процессам. Например, чтобы приостановить процесс, используйте `kill -STOP [pid]`. Для полного списка посмотрите `man 7 signal`.

- Используйте `nohup` или `disown`, чтобы процесс в фоне выполнялся бесконечно.

- Узнайте, какие процессы слушают порты через `netstat -lntp` или `ss -plat` (для TCP; добавьте `-u` для UDP).

- Используйте `lsof` для того, чтобы посмотреть открытые сокеты и файлы.

- Используйте `alias`, чтобы поименовать частоиспользуемые команды. Например, `alias ll='ls -latr'` создаст новое сокращение `ll`.

- В Баш скриптах используйте `set -x` для того, чтобы дебажить аутпут. Используйте строгие режимы везде, где возможно. Используйте `set -e` для того, чтобы прекращать выполнение при ошибках. Используйте `set -o pipefail` для того, чтобы строго относится к ошибкам (это немного глубокая тема). Для более сложных скриптов также используйте `trap`.

- В Баш-скриптах подоболочки (subshells) – удобный способ группировать команды. Один из самых распространенных примеров – временно передвинуться в другую рабочую директорию, вот так:
```bash
      # do something in current dir
      (cd /some/other/dir && other-command)
      # continue in original dir
```

- В Баше много типов пространства переменных. Проверить, существует ли переменная – `${name:?error message}`. Например, если Баш-скрипту нужен всего один аргумент, просто напишите `input_file=${1:?usage: $0 input_file}`. Арифметическая область видимости: `i=$(( (i + 1) % 5 ))`. Последовательности: `{1..10}`. Обрезка строк: `${var%suffix}` и `${var#prefix}`. Например, если `var=foo.pdf` тогда `echo ${var%.pdf}.txt` выведет `foo.txt`.

- Вывод любой команды можно сохранить в файлоподобный контекст по `<(some command)`. Например, сравнение локального файла `/etc/hosts с удалённым:
```sh
      diff /etc/hosts <(ssh somehost cat /etc/hosts)
```

- Знайте про *heredoc*-синтаксис в Баше, работает он так: `cat <<EOF ...`.

- В Баше перенаправляйте стандартный вывод, а также стандартные ошибки, вот так: `some-command >logfile 2>&1`. Зачастую, для того, чтобы убедится, что команда не оставит открытым файл, привязав его к открытому терминалу, считается хорошей практикой добавлять `</dev/null`.

- Используйте `man ascii` для хорошей ASCII таблицы, с *hex-* и десятичными значениями. Для информации по основным кодировкам полезны: `man unicode`, `man utf-8` и `man latin1`.

- Используйте `screen` или [`tmux`](https://tmux.github.io/) для того, чтобы иметь несколько экранов в одном терминале. Это особенно полезно, когда вы работаете с удаленным сервером, тогда Вы можете подключаться/отключаться от сессий. Более минималистичный подход для этого – использование `dtach`.

- В SSH полезно знать как сделать port tunnel с флагами `-L` и `-D` (и иногда `-R`). Например для того, чтобы зайти на сайт с удаленного сервера.

- Еще может быть полезно оптимизировать вашу SSH конфигурацию, например этот файл `~/.ssh/config` содержит настройки, которые помогают избегать потерянных подключений в некоторых сетевых окружениях. Используйте сжатие (которое полезно с scp через медленные подключения) и увеличьте количество каналов к одному серверу через этот конфиг, вот так:
```
      TCPKeepAlive=yes
      ServerAliveInterval=15
      ServerAliveCountMax=6
      Compression=yes
      ControlMaster auto
      ControlPath /tmp/%r@%h:%p
      ControlPersist yes
```

- Некоторые другие настройки SSH могут сильно повлиять на безопасность и должны меняться осторожно, например, для конкретной подсети или конкретной машины или в домашних сетях: `StrictHostKeyChecking=no`, `ForwardAgent=yes`

- Чтобы получить разрешения файла в восьмеричном виде, что полезно для конфигурации систем, но нельзя получить из `ls`, можно использовать что-то типа:
```sh
      stat -c '%A %a %n' /etc/timezone
```

- Для интерактивного выделения результатов других команд используйте [`percol`](https://github.com/mooz/percol) или [`fzf`](https://github.com/junegunn/fzf).

- Для работы с файлами, список которых дала другая команда (например, Git), используйте `fpp` ([PathPicker](https://github.com/facebook/PathPicker)).

- Чтобы быстро поднять веб-сервер в текущей директории (и поддерикториях), который доступен для всех в вашей сети, используйте:
`python -m SimpleHTTPServer 7777` (если у вас Python 2 и вы хотите открыть сервер на порту 7777) или `python -m http.server 7777` (для Python 3 и порта 7777).

- Чтобы выполнить определённую команду с привилегиями, используйте `sudo` (для рута) и `sudo -u` (для другого пользователя). Используйте `su` или `sudo bash`, чтобы запустить шелл от имени этого пользователя. Используйте `su -`, чтобы симулировать свежий логин от рута или другого пользователя.


## Процессинг файлов и информации

- Для того, чтобы найти файл в текущей директории, сделайте `find . -iname '*something*'`. Для того, чтобы искать файл по всей системе, используйте `locate something` (но не забывайте, что `updatedb` мог еще не проиндексировать недавно созданные файлы).

- Для основного поиска по содержимому файлов (более сложному, чем `grep -r`) используйте [`ag`](https://github.com/ggreer/the_silver_searcher).

- Для конвертации HTML в текст: `lynx -dump -stdin`

- Для конвертации разных типов разметки (HTML, Markdown и др.) попробуйте [`pandoc`](http://pandoc.org/).

- Если нужно работать с XML, есть старая, но хорошая утилита – `xmlstarlet`.

- Для работы с JSON используйте `jq`.

- Для работы с Excel и CSV-файлами используйте [csvkit](https://github.com/onyxfish/csvkit) (программа предоставляет команды `in2csv`, `csvcut`, `csvjoin`, `csvgrep` и т.д.)

- Для работы с Amazon S3 удобно работать с [`s3cmd`](https://github.com/s3tools/s3cmd) и [`s4cmd`](https://github.com/bloomreach/s4cmd) (последний работает быстрее). Для остальных сервисов Амазона используйте стандартный [`aws`](https://github.com/aws/aws-cli).

- Знайте про `sort` и `uniq`, включая флаги `-u` и `-d`, смотрите примеры ниже. Ещё попробуйте `comm`.

- Знайте про `cut`, `paste`, и `join` для работы с текстовыми файлами. Многие люди используют `cut`, забыв про `join`.

- Знайте о `wc`: для подсчёта переводов строк (`-l`), для символов – (`-m`), для слов – words (`-w`), для байтового подсчёта – (`-c`).

- Знайте про `tee` для копирования в файл из stdin и stdout, что-то типа `ls -al | tee file.txt`.

- Не забывайте, что Ваша локаль влияет на многие команды, включая порядки сортировки, сравнение и производительность. Многие дистрибутивы Linux автоматически выставляют `LANG` или любую другую переменную в подходящую для Вашего региона. Из-за этого результаты функций сортировки могут работать непредсказуемо. Рутины `i18n` могут значительно снизить производительность сортировок. В некоторых случаях можно полностью этого избегать (за исключением редких случаев), сортируя традиционно побайтово, для этого `export LC_ALL=C`.

- Знайте основы `awk` и `sed` для простых манипуляций с данными. Например, чтобы получить сумму всех чисел, которые находятся в третьей колонке текстового файла, можно использовать `awk '{ x += $3 } END { print x }'`. Скорее всего, это получится раза в 3 быстрее и раза в 3 проще, чем делать это в Питоне.

- Чтобы заменить все нахождения подстроки в одном или нескольких файлах:
```sh
      perl -pi.bak -e 's/old-string/new-string/g' my-files-*.txt
```

Для того, чтобы переименовать сразу много файлов по шаблону, используйте `rename`. Для сложных переименований может помочь [`repren`](https://github.com/jlevy/repren):

```sh
      # Восстановить бекапы из foo.bak в foo:
      rename 's/\.bak$//' *.bak
      # Полное переименование имён файлов, папок и содержимого файлов из foo в bar.
      repren --full --preserve-case --from foo --to bar .
```

- Используйте `shuf`, чтобы перемешать строки или выбрать случайную строчку из файла.

- Знайте флаги `sort`а. Для чисел используйте `-n`, для работы с человекочитаемыми числами используйте `-h` (например `du -h`). Знайте как работают ключи (`-t` и `-k`). В частности, не забывайте, что вам нужно писать `-k1,1` для того, чтобы отсортировать только первое поле; `-k1` - это сортировка, учитывая всю строчку. Также стабильная сортировка может быть полезной (`sort -s`). Например для того, чтобы отсортировать самое важное по второму полю, а второстепенное по первому, можно использовать sort -k1,1 | sort -s -k2,2`.

- Если вам когда-нибудь придётся написать код символа табуляции в терминале, например, для сортировки по табуляциям с флагом -t, используйте сокращение **ctrl-v** **[Tab]** или напишите `$'\t'`. Последнее лучше, потому что его можно скопировать.

- Стандартные инструменты для патчинга исходников это `diff` и `patch`. Также посмотрите на `diffstat` для просмотра статистики диффа. `diff -r` работает рекурсивно по всей директории. Используйте `diff -r tree1 tree2 | diffstat` для полной сводки изменений.

- Для бинарников используйте `hd` для простых hex-дампом, и `bvi` для двоичного изменения бинарников.

- `strings` (в связке с `grep` или чем-то похожим) помогает найти строки в бинарниках.

- Чтобы посмотреть разницу в бинарниках (дельта-кодирование): `xdelta3`.

- Для конвертирования кодировок используйте `iconv`. Для более сложных задач – `uconv`, он поддерживает некоторые сложные фичи Юникода. Например, эта команда переводит строки из файла в нижний регистр и убирает ударения (кои бывают, например, в испанском языке)

```sh
      uconv -f utf-8 -t utf-8 -x '::Any-Lower; ::Any-NFD; [:Nonspacing Mark:] >; ::Any-NFC; ' < input.txt > output.txt
```

- Для того, чтобы разбить файл на куски, используйте `split` (разбивает на куски по размеру), или `csplit` (по шаблону или регулярному выражению).

- Используйте `zless`, `zmore`, `zcat`, и `zgrep` для работы со сжатыми файлами.

## Системный дебаггинг

- Дле веб-дебаггинга используйте `curl` и `curl -I`, или их альтернативу - `wget`. Также есть более современные альтернативы, например [`httpie`](https://github.com/jakubroztocil/httpie).

- Чтобы получить информацию о диске/CPU/сети используйте `iostat`, `netstat`, `top` (или лучшую альтернативу `htop`) и особенно `dstat`. Хороший старт для того, чтобы понимать, что происходит в системе.

- Для более детальной информации используйте [`glances`](https://github.com/nicolargo/glances). Эта программа показывает сразу несколько разных статистик в одном окне терминала. Полезно, когда следите за сразу несколькими системами.

- Для того, чтобы следить за памятью, научитесь понимать `free` и `vmstat`. В частности, не забывайте, что кешированые значения ("cached" value) – это память, которую держит ядро и эти значения являются частью `free`.

- Дебаггинг Java – совсем другая рыбка, но некоторые манипуляции над виртуальной машиной Оракла, или любой другой, позволят вам использовать делать `kill -3 <pid>` и трассировать сводки стека и хипа (включая детали работы сборщика мусора, которые бывают очень полезными), и их можно дампнуть в stderr или логи.

- Используйте `mtr` для лучшей трассировки, чтобы находить проблемы сети.

- Для того, чтобы узнать, почему диск полностью забит, используйте `ncdu`, это сохраняет время по сравнению с тем же `du -sh *`.

- Для того, чтобы узнать, какой сокет или процесс использует интернет, используйте `iftop` или `nethogs`.

- `ab`, которая поставляется вместе с apache, полезна для быстрой и поверхностной проверки производительности веб-сервера. Для более серьезного лоад-тестинга используйте `siege`.

- Для более серьёзного дебаггинга сетей используйте `wireshark`, `tshark`, и `ngrep`.

- Знайте про `strace` и `ltrace`. Эти команды могут быть полезны, если программа падает или висит, и вы не знаете почему Или если вы хотите протестировать производительность программы. Не забывайте про возможность дебаггинга (`-c`) и возможность прицепиться к процессу по pid (`-p`).

- Не забывайте про `ldd` для проверки системных библиотек.

- Знайте как прицепиться к работающему процессу через `gdb` и получить трассировку стека.

- Используйте `/proc`. Иногда он невероятно полезен для отладки запущенных программ. Примеры: `/proc/cpuinfo`, `/proc/xxx/cwd`, `/proc/xxx/exe`, `/proc/xxx/fd/`, `/proc/xxx/smaps`.

- Когда дебажите что-то, что сломалось в прошлом, используйте `sar` – бывает очень полезно. Показывает историю CPU, памяти, сети и т.д.

- Для анализа более сложных систем и производительности посмотрите на `stap` ([SystemTap](https://sourceware.org/systemtap/wiki)), [`perf`](http://en.wikipedia.org/wiki/Perf_(Linux)), и [`sysdig`](https://github.com/draios/sysdig).

- Узнайте, какая у вас операционка, через `uname` or `uname -a` (основная Unix-информация/информация о ядре) или `lsb_release -a` (информация о дистрибутиве).

- Используйте `dmesg`, когда что-то ведет себя совсем странно (например, железо или драйвера).

## В одну строчку

Давайте соберем все вместе и напишем несколько команд:

- Это довольно круто, что можно найти множественные пересечения файлов, соединить отсортированные файлы и посмотреть разницу в нескольких файлов через `sort`/`uniq`. Это быстрый подход и работает на файлах любого размера (включая многогигабайтные файлы). (Сортировка не ограничена памятью, но возможно вам придется добавить `-T`, если `/tmp` находится на небольшом логическом диске). Еще посмотрите то, что было сказано выше о `LC_ALL`. Флаг сортировки `-u` не используется ниже, чтобы было понятнее:
```sh
      cat a b | sort | uniq > c        # c is a union b
      cat a b | sort | uniq -d > c     # c is a intersect b
      cat a b b | sort | uniq -u > c   # c is set difference a - b
```

- Используйте `grep . *` для того, чтобы посмотреть содержимое всех файлов в директории. Особенно полезно, когда у вас много конфигов типа `/sys`, `/proc`, `/etc`.


- Получить сумму всех чисел, которые находятся в третьей колонке текстового файла. (Скорее всего, это раза в 3 быстрее и раза в 3 проще, чем делать это в Питоне):
```sh
      awk '{ x += $3 } END { print x }' myfile
```

- Если вам нужно посмотреть размеры и даты создания древа файлов используйте:
```sh
      find . -type f -ls
```

Это почти как рекурсивная `ls -l`, но выглядит более читабельно чем `ls -lR`:

- Используйте `xargs` (или `parallel`). Это очень мощная штука. Обратите внимание, что Вы можете контролировать количество команд на каждую строку, а так же параллельность. Если Вы не уверены, что делаете что-то правильно, начните с `xargs echo`. Еще `-I{}` – полезная штука. Примеры:
```sh
      find . -name '*.py' | xargs grep some_function
      cat hosts | xargs -I{} ssh root@{} hostname
```

- Давайте представим, что у нас есть какой-то текстовый файл, например лог какого-то сервера и на каких-то строках появляется значение, строки с которым нам интересны. Например, `acct_id`. Давайте подсчитаем, сколько таких запросов в нашем логе:
```sh
      cat access.log | egrep -o 'acct_id=[0-9]+' | cut -d= -f2 | sort | uniq -c | sort -rn
```

- Запустите этот скрипт, чтобы получить случайный совет из этой инструкции:
```sh
      function taocl() {
        curl -s https://raw.githubusercontent.com/jlevy/the-art-of-command-line/master/README.md |
          pandoc -f markdown -t html |
          xmlstarlet fo --html --dropdtd |
          xmlstarlet sel -t -v "(html/body/ul/li[count(p)>0])[$RANDOM mod last()+1]" |
          xmlstarlet unesc | fmt -80
      }
```


## Сложно, но полезно

- `expr`: для выполнения арифметических и булевых операций, а также регулярных выражений

- `m4`: простенький макро-процессор

- `yes`: вывод строки в бесконечном цикле

- `cal`: классный календарь

- `env`: для того, чтобы выполнить команду (полезно в Bash-скриптах)

- `printenv`: показать переменные окружения (полезно в скриптах или дебаггинге)

- `look`: найти английские слова (или строки) в файле

- `cut `, `paste` и `join`: манипуляции с данными

- `fmt`: форматирование параграфов в тексте

- `pr`: отформатировать текст в страницы/колонки

- `fold`: (обернуть) ограничить длину строк в файле

- `column`: форматировать текст в колонки или таблицы

- `expand` и `unexpand`: конвертация между табами и пробелами

- `nl`: добавить номера строк

- `seq`: вывести последовательность чисел

- `bc`: калькулятор

- `factor`: возвести числа в степень

- `gpg`: зашифровать и подписать файлы

- `toe`: таблица терминалов terminfo с описанием

- `nc`: дебаггинг сети и передачи данных

- `socat`: переключатель сокетов и перенаправление tcp-портов (похоже на `netcat`)

- `slurm`: визуализация трафика сети

- `dd`: перенос информации между блочными устройствами

- `file`: узнать тип файла

- `tree`: показать директории и поддиректории в виде дерева, как `ls`, но рекурсивно

- `stat`: информация о файле

- `tac`: вывести файл посимвольно наоборот ("ласипан")

- `shuf`: случайная выборка строк из файла

- `comm`: построчно сравнить отсортированные файлы

- `pv`: мониторинг прогресса прохождения информации через пайп

- `hd`, `hexdump`, `xxd`, `biew`: hex-дамп и редактирование бинарников

- `strings`: найти текст в бинарниках

- `tr`: манипуляция с char (символьным типом)

- `iconv` и `uconv`: конвертация кодировок

- `split` и `csplit`: разбить файлы

- `sponge`: прочитать весь инпут перед тем, как его записать. Полезно, когда читаешь из того же файла, куда записываешь. Например, вот так: `grep -v something some-file | sponge some-file`

- `units`: конвертер. Метры в километры, версты в пяди (смотрите `/usr/share/units/definitions.units`)

- `7z`: архиватор с высокой степенью сжатия

- `ldd`: показывает зависимости программы от системных библиотек

- `nm`: получаем названия всех функций, которые определены в .o или .a

- `ab`: бенчмаркинг веб-серверов

- `strace`: отладка системных вызовов

- `mtr`: лучшая трассировка для дебаггинга сети

- `cssh`: несколько терминалов в одном UI

- `rsync`: синхронизация файлов и папок через SSH

- `wireshark` и `tshark`: перехват пакетов и дебаг сети

- `ngrep`: grep для слоя сети (network layer)

- `host` и `dig`: узнать DNS

- `lsof`: процессинг дескрипторов и информация о сокетах

- `dstat`: полезная статистика ОС

- [`glances`](https://github.com/nicolargo/glances): высокоуровневая статистика по многим подсистемам

- `iostat`: статистика процессора и использования жёсткого диска

- `htop`: улучшенная версия `top`

- `last`: история логинов в систему

- `w`: под каким пользователем вы сидите

- `id`: информация о пользователе/группе

- `sar`: история системной статистики

- `iftop` или `nethogs`: использование сети конкретным сокетом или процессом

- `ss`: статистика сокетов

- `dmesg`: ошибки загрузки и ошибки системы

- `hdparm`: манипуляции с SATA/ATA

- `lsb_release`: информация о дистрибутиве Linux

- `lsblk`: cписок блочных устройств компьютера: дерево ваших дисков и логических дисков

- `lshw`, `lscpu`, `lspci`, `lsusb`, `dmidecode`: информация о железе, включая CPU, BIOS, RAID, графику, девайсы, и т.д.

- `fortune`, `ddate`, и `sl`: хм, не знаю, будут ли вам "полезны" веселые цитатки и поезда, пересекающие ваш терминал :)

## MacOS X only

Некоторые вещи, подходящие *только* для Мака.

- Системы управлением пакетами – `brew` (Homebrew) и `port` (MacPorts). Они могут быть использованы для того, чтобы установить большинство программ, упомянутых в этом документе.

- Копируйте выдачу консольных программ в десктопные через `pbcopy` и вставляйте входные данные через `pbpaste`.

- Для того, чтобы открыть файл или десктопную программу типа Finder, используйте `open`. Вот так: `open -a /Applications/Whatever.app`.

- Spotlight: Ищите файлы в консоли, через `mdfind`, и смотрите метадату (например EXIF информацию фотографий) через `mdls`.

- Не забывайте, что MacOS основан на BSD Unix и многие команды (например `ps`, `ls`, `tail`, `awk`, `sed`) имеют небольшие различия с линуксовыми. Это обусловлено влянием `UNIX System V` и `GNU Tools`. Разницу можно заметить, увидев заголовок "BSD General Commands Manual." в манах программ. В некоторых случаях, на Мак можно поставить GNU-версии программ, например `gawk` и `gsed`. Когда пишите кроссплатформенные Bash-скрипты, старайтесь избегать использовать команды, которые могут различаться (например, лучше используйте Python или `perl`), или тщательно все тестируйте.

## Больше информации по теме

- [awesome-shell](https://github.com/alebcay/awesome-shell): Дополняемый список инструментов и ресурсов для командной строки.
- [Strict mode](http://redsymbol.net/articles/unofficial-bash-strict-mode/) Для того, чтобы писать шелл-скрипты лучше.


## Дисклеймер

За небольшим исключением, весь код написан так, чтобы другие его смогли прочитать.

Кому много дано, с того много и спрашивается. Тот факт, что что-то может быть написано на Баше, вовсе не означает, что оно должно быть на нём написано. ;)


## Лицензия

[![Creative Commons License](https://i.creativecommons.org/l/by-sa/4.0/88x31.png)](http://creativecommons.org/licenses/by-sa/4.0/)

Оригинальная работа и перевод на русский язык распространяется под лицензией [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).
