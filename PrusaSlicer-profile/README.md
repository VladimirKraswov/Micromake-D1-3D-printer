**Минимальный профиль PrusaSlicer для кастомного Micromake D1**

Чтобы отредактировать шаблон профиля:

```bash
mate "/Applications/Original Prusa Drivers/PrusaSlicer.app/Contents/Resources/profiles/Micromake_D1.ini"
```

Чтобы очистить старые настройки PrusaSlicer:

```bash
cd ~/Library/Application\ Support/PrusaSlicer
rm -rf *
```

Для отладки запустите PrusaSlicer с повышенным уровнем логирования:

```bash
"/Applications/Original Prusa Drivers/PrusaSlicer.app/Contents/MacOS/PrusaSlicer" --loglevel 4
```
