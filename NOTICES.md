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
| opencode-pty (демон терминала) | `server/opencode-pty` | 0.4.0 | MIT | извлечён из `server/opencode` того же пакета `Push261104` |
| Termux bootstrap (userland) | `bootstrap.zip` | `bootstrap-2026.11.01-r1+apt.android-7` | GPL-2.0, GPL-3.0, LGPL, BSD, MIT | `termux/termux-packages` |
| proot | `bin/proot`, `libexec/proot/loader`, `libexec/proot/loader32` | 5.1.107.102 | GPL-2.0 | `termux/proot` |
| talloc | `lib/libtalloc.so.2` | 2.5.0 | LGPL-3.0-or-later | `termux/libtalloc` |
| libandroid-shmem | `lib/libandroid-shmem.so` | 0.7 | BSD-3-Clause | `termux/libandroid-shmem` |
| Иконки интерфейса | `app/res/drawable/` | — | Apache-2.0 | Material Icons, <https://fonts.google.com/icons> |

## Тексты лицензий и исходники

| Компонент | Лицензия | Исходники / полный текст |
|---|---|---|
| opencode | MIT, Copyright SST / opencode contributors | <https://github.com/sst/opencode>; сборка под Android — <https://github.com/Hope2333/opencode-termux> |
| bootstrap | лицензии пакетов Termux | <https://github.com/termux/termux-packages> (поле `LICENSE` в `build.sh` каждого пакета) |
| proot | GPL-2.0, Copyright Cédric Vincent, STMicroelectronics | <https://github.com/termux/proot>, текст — `COPYING` |
| talloc | LGPL-3.0-or-later, Copyright Andrew Tridgell, Samba Team | <https://github.com/samba-team/samba> (каталог `lib/talloc`) |
| libandroid-shmem | BSD-3-Clause, Copyright (c) 2013 Sergii Pylypenko, (c) 2017 Fredrik Fornwall | <https://github.com/termux/libandroid-shmem> |
| Иконки интерфейса | Apache-2.0 | <https://fonts.google.com/icons> |

Код самого приложения (`app/java/com/spz/octomobile/**`, `tools/**`) — оригинальный,
лицензия MIT, см. `LICENSE`. Лицензия MIT не распространяется на сторонние
бинарники из таблиц выше: у каждого своя.
