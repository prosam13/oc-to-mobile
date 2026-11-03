# Сторонние компоненты и лицензии

OC to Mobile не содержит собственного сервера и не пересобирает opencode. Приложение
распространяет готовые сторонние бинарники в неизменном виде и запускает их на
устройстве. Ниже — что именно поставляется, откуда взято и под какой лицензией.

Версии и контрольные суммы зафиксированы в `app/assets/runtime/arm64-v8a/payload.json`
(генерируется `tools/fetch_runtime.py`, поле `sources`).

## Состав payload

| Компонент | Файл в payload | Версия | Лицензия | Источник |
|---|---|---|---|---|
| opencode (сервер + агент) | `server/opencode` | 2.2.5-2 | MIT | `Hope2333/opencode-termux`, релиз `Push261104` (сборка opencode под Android/bionic) |
| crhandler (шим `close_range`) | `server/libopencode-crhandler.so` | из того же пакета | MIT | там же |
| opencode-pty (демон терминала) | `server/opencode-pty` | 0.4.0 | MIT | извлечён из `server/opencode` того же пакета `Push261104` (встроенный ресурс `/$bunfs/root/opencode-pty-…`) |
| Termux bootstrap (userland) | `bootstrap.zip` | `bootstrap-2026.11.01-r1+apt.android-7` | набор лицензий, см. ниже | `termux/termux-packages` |
| proot | `bin/proot`, `libexec/proot/loader`, `libexec/proot/loader32` | 5.1.107.102 | GPL-2.0 | пакет `proot` из `packages.termux.dev` (`termux/proot`) |
| talloc | `lib/libtalloc.so.2` | 2.5.0 | LGPL-3.0-or-later | пакет `libtalloc` из `packages.termux.dev` (`termux/libtalloc`) |
| libandroid-shmem | `lib/libandroid-shmem.so` | 0.7 | BSD-3-Clause | пакет `libandroid-shmem` из `packages.termux.dev` (`termux/libandroid-shmem`) |

Актуальный `app/assets/runtime/arm64-v8a/payload.json` содержит 9 файлов: `bootstrap.zip`,
proot с лоадерами, talloc, libandroid-shmem, `server/opencode`,
`server/libopencode-crhandler.so` и `server/opencode-pty`.

## opencode-pty (демон терминала)

Демон терминала opencode (Rust, протокол 7, версия 0.4.0) — часть пакета
`opencode_2.2.5-2_aarch64.deb` из `Hope2333/opencode-termux` (`Push261104`),
лицензия MIT (Copyright SST / opencode contributors). В самом сервере он лежит
встроенным ресурсом; из-за того что на Android/bionic виртуальный путь
`/$bunfs/…` не резолвится, распаковка молча падала и терминал не работал.
Поэтому ELF извлекается из `server/opencode` побайтно (offset 336379102,
длина 2989352, sha256
`1e5bdbc5c41967dae6a9fc6a0dbc1f8db28cb87b0002263dd933eae7ac1c4d8e`) и
поставляется отдельным файлом `server/opencode-pty`. `ServerProcess` передаёт его
демону через `OPENCODE_PTY_BIN` (заданный путь opencode проверяет раньше
самораспаковки), `XDG_STATE_HOME` и `OPENCODE_PTY_SOCKET_DIR` — последний нужен,
потому что создание unix-сокета внутри `files/home/...` на Android даёт
`EACCES (os error 13)`.

Распаковка при сборке (`tools/fetch_runtime.py` → `add_file("server/opencode-pty")`,
исходник берётся из `_work/stage0/`) и установка на устройство
(`RuntimeInstaller`) не изменяют бинарник; права `0755` выставляются при
раскладке. Патч из PR #40 (`TanakaLun/opencode-termux`) не применён — файл
берётся байт-в-байт.

## opencode

Copyright SST / opencode contributors. Лицензия MIT: разрешено использование,
копирование, изменение и распространение при сохранении текста лицензии и
уведомления об авторских правах; ПО поставляется «как есть».

Исходники: <https://github.com/sst/opencode>. Сборка под Android — из
<https://github.com/Hope2333/opencode-termux>.

`libopencode-crhandler.so` подменяет вызов `close_range`, который блокируется
seccomp-политикой Android и убивает процесс Bun сигналом SIGSYS. Это минимальная
совместимость, а не изменение поведения агента.

## Termux bootstrap

`bootstrap.zip` — набор утилит Termux (`bash`, `coreutils`, `sed`, `grep`, `git`,
`nodejs` и прочие), собранных проектом Termux. Каждый бинарник распространяется
под своей лицензией: преимущественно GPL-2.0, GPL-3.0, LGPL, BSD и MIT.
Полные тексты лицензий и метаданные пакетов — в
<https://github.com/termux/termux-packages> (каталог `packages/`, поле `LICENSE`
в `build.sh` каждого пакета).

Архив поставляется без изменений; распаковка и раскладка симлинков выполняются на
устройстве при первом запуске.

## proot

Copyright Cédric Vincent, STMicroelectronics. Лицензия GPL-2.0:
<https://github.com/termux/proot>.

proot запускается только для шелла агента (изоляция файловой системы через `-b`
привязки). Сам сервер opencode запускается в host namespace, без proot.

GPL-2.0 требует предоставления исходного кода производных работ. Мы не изменяли
proot и запускаем его как отдельный исполняемый файл, поэтому исходники
предоставляются по ссылке выше. Полный текст лицензии — `COPYING` в том же
репозитории.

## talloc

Copyright Andrew Tridgell, Samba Team. Лицензия LGPL-3.0-or-later:
<https://github.com/samba-team/samba> (каталог `lib/talloc`). Бинарник
используется без изменений и динамически линкуется proot.

## libandroid-shmem

Copyright (c) 2013 Sergii Pylypenko, (c) 2017 Fredrik Fornwall. Лицензия
BSD-3-Clause: <https://github.com/termux/libandroid-shmem>.

При распространении в бинарном виде требуется сохранять текст уведомления об
авторских правах и условия лицензии — этот файл выполняет это требование.

## Иконки интерфейса

Иконки в `app/res/drawable/` нарисованы для этого проекта по мотивам Material
Icons (лицензия Apache-2.0, <https://fonts.google.com/icons>).

## Что делает само приложение

Код OC to Mobile (`app/java/com/spz/octomobile/**`, `tools/**`) — оригинальный, написан для
этого проекта, лицензия MIT (см. `LICENSE`). Лицензия MIT не распространяется на
сторонние бинарники из таблицы выше: у каждого своя.
