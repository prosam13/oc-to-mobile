# Сторонние компоненты и лицензии

OC to mobile не содержит собственного сервера и не пересобирает opencode. Приложение
распространяет готовые сторонние бинарники в неизменном виде и запускает их на
устройстве. Ниже — что именно поставляется, откуда взято и под какой лицензией.

Версии и контрольные суммы зафиксированы в `app/assets/runtime/arm64-v8a/payload.json`
(генерируется `tools/fetch_runtime.py`, поле `sources`).

## Состав payload

| Компонент | Файл в payload | Версия | Лицензия | Источник |
|---|---|---|---|---|
| opencode (сервер + агент) | `server/opencode` | 2.1.4 | MIT | `Hope2333/opencode-termux`, релиз `Push261026` (сборка opencode под Android/bionic) |
| crhandler (шим `close_range`) | `server/libopencode-crhandler.so` | из того же пакета | MIT | там же |
| Termux bootstrap (userland) | `bootstrap.zip` | `bootstrap-2026.10.25-r1+apt.android-7` | набор лицензий, см. ниже | `termux/termux-packages` |
| proot | `bin/proot`, `libexec/proot/loader`, `libexec/proot/loader32` | 5.1.107.101 | GPL-2.0 | пакет `proot` из `packages.termux.dev` (`termux/proot`) |
| talloc | `lib/libtalloc.so.2` | 2.5.0 | LGPL-3.0-or-later | пакет `libtalloc` из `packages.termux.dev` (`termux/libtalloc`) |
| libandroid-shmem | `lib/libandroid-shmem.so` | 0.7 | BSD-3-Clause | пакет `libandroid-shmem` из `packages.termux.dev` (`termux/libandroid-shmem`) |
| librust_pty (bun-pty, bionic) | `lib/librust_pty_arm64.so` | graft из PR #40, sha256 `58aeeb46…1c2d` | MIT | `TanakaLun/opencode-termux`, PR «fix(pty): graft bionic librust_pty», сборка `tools/bun-pty-embed/build-embed.sh` |

## librust_pty (bun-pty, bionic)

Встроенная в серверный bunfs библиотека bun-pty собрана под glibc и на Android не
открывается (`libpthread.so.0 not found`), из-за чего в Web UI не создаётся
терминал (`POST /api/pty` → 500, `undefined is not an object (evaluating
'i.symbols')`). Поэтому рядом с сервером лежит bionic-вариант той же библиотеки
(8 экспортов `bun_pty_*`), а `ServerProcess` указывает на него через
`BUN_PTY_LIB`. Файл распространяется без изменений, sha256
`58aeeb4647cdcdb7e4d9f855ebaa75339ab934039d7ddd977a0be5a3b5cf1c2d`. Логика
замены и сборки — `tools/transplant/swap_pty.py` и `tools/bun-pty-embed/` в
указанном репозитории.

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

Код OC to mobile (`app/java/com/spz/octomobile/**`, `tools/**`) — оригинальный, написан для
этого проекта, лицензия MIT (см. `LICENSE`). Лицензия MIT не распространяется на
сторонние бинарники из таблицы выше: у каждого своя.
