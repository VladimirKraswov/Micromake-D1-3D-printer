## Важная заметка:

Оригинальный Z-пробник у Micromake — полный отстой. Его конструкция и трение пластиковых деталей приводят к случайной погрешности зондирования до 0,2 мм по разным участкам стола. Даже лучший алгоритм калибровки не выровняет поверхность, если исходные данные такие “грязные”.

Решение этой проблемы — устройство за \$1,5, которое крепится прямо на сопло:

[https://aliexpress.com/item/32838312776.html](https://alitems.com/g/1e8d1144949a590a4ec116525dc3e8/?ulp=https%3A%2F%2Faliexpress.com%2Fitem%2F32838312776.html)

![zprobe](https://user-images.githubusercontent.com/1763243/80029227-27eb3e80-84ef-11ea-9c88-dffa44bcf45f.png)

Это гибкая кнопка толщиной 0,2 мм, которая щёлкает, как только сопло касается стола. Она мягкая и не деформирует экструдер, как заводской механизм.

## Важная заметка № 2:

Это руководство больше не поддерживается. Я перешёл со стоковой прошивки Repetier на Marlin 2.0, в которой есть отличная функция «Unified Bed Leveling» (единое выравнивание стола). Она работает как волшебство: [https://marlinfw.org/docs/features/unified\_bed\_leveling.html](https://marlinfw.org/docs/features/unified_bed_leveling.html)

---

# Патчинг и установка прошивки Micromake D1 (Repetier)

**Внимание!** В примерах я использую плату Micromake первого поколения, настроенную на 32 микрошага с помощью перемычки. Ваши настройки могут отличаться — проверьте их внимательно.

Плата принтера — по сути Arduino Mega с драйверами двигателей и MOSFET для нагревателя и стола. Её можно перепрограммировать через USB.

## Установка инструментов

1. Скачайте и установите Arduino IDE: [https://www.arduino.cc/en/Main/Software](https://www.arduino.cc/en/Main/Software) (Windows Installer).
2. Загрузите прошивку Repetier: [https://github.com/repetier/Repetier-Firmware](https://github.com/repetier/Repetier-Firmware) (базовые файлы).
3. Скачайте патчи Micromake: [https://drive.google.com/drive/folders/0B1DQUrzkDP-tNDU0NXhVcGhlc0k](https://drive.google.com/drive/folders/0B1DQUrzkDP-tNDU0NXhVcGhlc0k) (`Micromake D1.zip`).
4. Распакуйте `Micromake D1.zip` в папку `C:\Repetier`.
5. Откройте `Repetier.ino` в Arduino IDE.
6. Подключите плату по USB.
7. В меню **Tools → Board** выберите **Arduino/Genuino Mega or Mega 2560**.
8. В **Tools → Port** выберите COM-порт платы.
9. Нажмите **Ctrl+R** для проверки компиляции. В конце должно появиться:

   ```
   Sketch uses 137,236 bytes (54%) of program storage space. Maximum is 253,952 bytes.
   Global variables use 6,984 bytes (85%) of dynamic memory, leaving 1,208 bytes for local variables. Maximum is 8,192 bytes.
   ```

## Патчинг прошивки

### 1. Исправление G30 (Commands.cpp)

Вкладка `Commands.cpp`, найдите блок:

```cpp
case 30: { // G30 set Z0
    uint8_t p = (com->hasP() ? (uint8_t)com->P : 3);
```

Измените `: 3` на `: 2`:

```cpp
    uint8_t p = (com->hasP() ? (uint8_t)com->P : 2);
```

Это необходимо для совместимости с OpenDACT.

### 2. Дополнительные правки (Configuration.h)

#### STARTUP\_GCODE

```cpp
#define STARTUP_GCODE "M115\nM119\nG28\nM140 S115\nM105 X0\n"
```

* `M115` — вывод версии прошивки
* `M119` — проверка концевиков
* `G28` — homing всех осей
* `M140 S115` — прогрев стола до 115 °C
* `M105 X0` — опрос термисторов

#### Z\_PROBE\_REPETITIONS

```cpp
#define Z_PROBE_REPETITIONS 3
```

Усреднение 3 измерений вместо 1.

#### Точки зондирования

```cpp
#define Z_PROBE_X1 49.24
#define Z_PROBE_Y1 8.682
#define Z_PROBE_X2 -32.139
#define Z_PROBE_Y2 38.302
#define Z_PROBE_X3 -8.682
#define Z_PROBE_Y3 -49.24
```

#### BEEPER

```cpp
#define BEEPER_SHORT_SEQUENCE 1,1
#define BEEPER_LONG_SEQUENCE 2,2
```

#### Подогрев стола

```cpp
#define HAVE_HEATED_BED 1
```

#### Длина диагонали дельты

```cpp
#define DELTA_DIAGONAL_ROD 217
```

#### Микрошаги и шаги на оборот

```cpp
#define STEPS_PER_ROTATION 200
#define MICRO_STEPS 32
```

#### Текст заставки

```cpp
#define UI_PRINTER_NAME "Micromake 2.0.?"
#define UI_PRINTER_COMPANY "www.micromake.org"
```

## Загрузка и проверка

1. Сохраните или очистите EEPROM (см. HOWTO по очистке).
2. Нажмите **Ctrl+U** для загрузки.
3. Проверьте движение экструдерной головки — при ошибках скоростей пересмотрите MICRO\_STEPS и steps/mm в EEPROM.

## Возврат к заводской прошивке

Загрузите стоковую прошивку через CURA — она сразу восстановит исходные настройки.

*Дальнейшая калибровка — в [руководстве по OpenDACT](https://github.com/Bougakov/Micromake-D1-3D-printer/blob/master/Calibrating%20Micromake%20D1%20with%20OpenDACT.md).*
