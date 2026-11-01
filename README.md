# OC to mobile

**[Русский](#русский) · [English](#english)**

---

## Русский

**Полноценный ИИ-агент opencode прямо на телефоне — и одновременно клиент opencode с компьютера.**

Это Android-приложение, в котором работает настоящий opencode: тот же агент, те же инструменты, тот же терминал, что и на компьютере, — только локально на вашем телефоне. Агент видит и правит файлы устройства, выполняет команды, общается с моделями по вашим ключам. Всё считается и хранится на самом телефоне: без компьютера, аккаунтов, облака и телеметрии.

Если opencode уже запущен на компьютере, приложение можно подключить к нему и работать с теми же сессиями с телефона — интерфейс одинаковый.

### Что умеет

- **Работать полностью на телефоне** — агент читает и правит ваши файлы, запускает команды, ставит задачи. Ничего дополнительно устанавливать не нужно.
- **Не останавливаться в фоне** — свернули приложение или заблокировали экран, задача продолжается, а когда агент закончит, придёт уведомление. Отдельное уведомление приходит, когда агент просит разрешение.
- **Показывать терминал** — можно запускать команды вручную.
- **Работать с файлами телефона** — вы сами выбираете рабочую папку, включая общую память устройства.
- **Подключаться к opencode на компьютере** — если там запущен сервер, добавьте его на стартовом экране и работайте с теми же сессиями.
- **Два языка интерфейса** — русский и английский: язык подхватывается из системы, а на стартовом экране есть переключатель RU / EN.

### Установка

1. Скачайте APK со страницы [Releases](https://github.com/prosam13-ui/oc-to-mobile/releases/latest).
2. Разрешите установку из этого источника, если Android спросит.
3. Откройте приложение и нажмите «Подключиться».

| | |
|---|---|
| Android | 9.0 и новее |
| Процессор | arm64 (все современные телефоны) |
| Размер APK | ~134 МБ |
| Свободное место | ~1 ГБ — при первом запуске приложение распаковывает свои компоненты |

Первый запуск занимает до минуты, дальше всё открывается сразу.

**Предупреждение при установке.** Android может сообщить, что приложение «создано для более ранней версии Android», или что оно из неизвестного источника. Это ожидаемо: приложение не распространяется через Google Play. Выбирайте «Всё равно установить».

### Известные ограничения

- **Только телефоны на arm64.** На устройствах с другим процессором приложение не запустится.
- **Не делится компонентами с Termux** — приложение использует свои, изолированные.
- **Энергосбережение.** На прошивках вроде MIUI/HyperOS система любит останавливать фоновые приложения. Если задачи обрываются, разрешите приложению работу в фоне и снимите для него ограничения батареи.

### Лицензии

Код приложения — MIT, см. `LICENSE`. Сторонние компоненты и их лицензии перечислены в `NOTICES.md`.

---

## English

**A full AI agent — opencode — right on your phone, and also a client for opencode on your computer.**

This Android app runs the real opencode locally: the same agent, the same tools, the same terminal you get on a desktop, but on your phone. The agent reads and edits files on the device, runs commands and talks to models with your own keys. Everything runs and stays on the phone — no computer, no accounts, no cloud, no telemetry.

If opencode is already running on your computer, you can connect the app to it and work with the same sessions from your phone — the interface is identical.

### What it does

- **Works entirely on the phone** — the agent reads and edits your files, runs commands and does the work. Nothing else to install.
- **Keeps going in the background** — the task continues after you switch apps or lock the screen, and you get a notification when the agent finishes. A separate notification appears when the agent asks for permission.
- **Gives you a terminal** — run commands yourself when you need to.
- **Works with your phone's files** — you choose the working folder, including shared storage.
- **Connects to opencode on your computer** — add the server on the start screen and work with the same sessions.
- **Two interface languages** — Russian and English: the language follows your system, and there is an RU / EN switch on the start screen.

### Installation

1. Download the APK from the [Releases](https://github.com/prosam13-ui/oc-to-mobile/releases/latest) page.
2. Allow installation from this source if Android asks.
3. Open the app and tap "Connect".

| | |
|---|---|
| Android | 9.0 or newer |
| CPU | arm64 (all modern phones) |
| APK size | ~134 MB |
| Free storage | ~1 GB — the app unpacks its components on first launch |

The first launch takes up to a minute; after that everything opens instantly.

**Installation warning.** Android may say the app "was built for an older version of Android", or that it comes from an unknown source. This is expected: the app is not distributed through Google Play. Choose "Install anyway".

### Known limitations

- **arm64 phones only.** The app will not run on devices with a different CPU architecture.
- **Does not share components with Termux** — the app uses its own isolated copy.
- **Battery optimisation.** ROMs like MIUI/HyperOS tend to stop background apps. If tasks get interrupted, allow the app to run in the background and remove its battery restrictions.

### License

The app code is MIT, see `LICENSE`. Third-party components and their licenses are listed in `NOTICES.md`.
