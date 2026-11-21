# MokomailOS

Дружелюбный Linux-дистрибутив на базе Debian 13 с оформлением в духе Material You.
Живая система с флешки/ISO и установка на диск.

## Что уже есть в этом каркасе

- Сборка live-ISO (BIOS + UEFI) на Debian 13 «trixie»
- Рабочий стол KDE Plasma 6, светлая тема `Mokomail` с акцентом Material 3
- Своё имя системы: `MokomailOS 1.0`, hostname `mokomailos`, пользователь live-сессии `mokomail`
- Обои и логотип (SVG, генерируются из кода)
- Первичная настройка при первом входе: тема, акцент, обои, раскладка us+ru, ярлык установщика
- Установщик Calamares в меню приложений

## Сборка ISO через GitHub Actions

1. Создать на GitHub пустой репозиторий `mokomailos` (можно Public — тогда минуты сборки бесплатны).
2. Запушить этот проект:

   ```sh
   git remote add origin https://github.com/<логин>/mokomailos.git
   git branch -M main
   git push -u origin main
   ```

3. Открыть вкладку **Actions** → **Build MokomailOS ISO** → **Run workflow**.
4. Через 30–60 минут скачать артефакт `mokomailos-iso` — внутри `mokomailos-amd64.hybrid.iso`.

Локально ничего ставить не нужно: сборка идёт в контейнере `debian:13` на раннере GitHub.

## Как запустить

### Виртуалка

- **VirtualBox**: создать ВМ (Linux / Debian 64-bit, 4 ГБ RAM, 20 ГБ диск, EFI выключен),
  подключить ISO как оптический диск, запустить.
- **QEMU** (портативный, без установки):

  ```sh
  qemu-system-x86_64 -m 4096 -smp 2 -cdrom mokomailos-amd64.hybrid.iso
  ```

### Реальный ПК

Записать ISO на флешку (Rufus в режиме DD или balenaEtcher), загрузиться с неё.
Образ гибридный: работает и с BIOS, и с UEFI.

## Структура

```
iso/
├── auto/
│   ├── config                  # параметры lb config
│   └── build                   # lb build
└── config/
    ├── package-lists/          # список пакетов
    └── includes.chroot/        # файлы, попадающие в систему
        ├── etc/                # os-release, lsb-release, sddm, autostart
        ├── usr/local/bin/      # скрипт первичной настройки
        └── usr/share/          # обои, иконки, цветовая схема
.github/workflows/build-iso.yml # сборка ISO в облаке
```

## План

- [x] Вечер 1 — каркас, брендинг, live-ISO
- [ ] Вечер 2 — наполнение: тема, панель, набор приложений, приветственное окно
- [ ] Вечер 3 — установщик: брендинг Calamares, установка на диск, загрузка на реальном ПК

## Заметки

- В live-сессии включён автовход пользователя `mokomail` (X11-сессия — надёжнее в виртуалках).
  При установке на диск этот автовход нужно убирать — сделаем вместе с настройкой Calamares.
- ISO получается примерно 2.5–3.5 ГБ.
