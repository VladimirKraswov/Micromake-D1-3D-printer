Часть вторая. Продолжение с “[Patching and installing Micromake D1 firmware (Repetier)](https://github.com/Bougakov/Micromake-D1-3D-printer/blob/master/Installing%20custom%20firmware%.md)”

# Важная заметка:

Стоковый Z-пробник (Z-probe) у Micromake — полный отстой. Из-за конструкции и трения пластиковых деталей погрешность при зондировании разных участков платформы может достигать 0,2 мм. Даже лучший алгоритм калибровки не выровняет стол, если исходные данные настолько “грязные”.

Решение — вот это устройство за \$1.5, которое крепится прямо на сопло:

[https://aliexpress.com/item/32838312776.html](https://alitems.com/g/1e8d1144949a590a4ec116525dc3e8/?ulp=https%3A%2F%2Faliexpress.com%2Fitem%2F32838312776.html)

![zprobe](https://user-images.githubusercontent.com/1763243/80029227-27eb3e80-84ef-11ea-9c88-dffa44bcf45f.png)

По сути, это гибкая кнопка толщиной 0,2 мм, которая срабатывает, как только сопло касается стола. Она мягкая и не “гнёт” узел, как оригинальный механизм.

# Важная заметка № 2:

Этот HOWTO больше не поддерживается. Я перешёл со стоковой прошивки Repetier на Marlin 2.0, в которой есть замечательная функция «Unified Bed Leveling» — она работает просто волшебно.

# Калибровка Micromake D1 с помощью утилиты OpenDACT

Скачать OpenDACT можно здесь: [http://forum.seemecnc.com/viewtopic.php?f=36\&t=8698](http://forum.seemecnc.com/viewtopic.php?f=36&t=8698). Вам нужна версия `3.0.1A`. Распакуйте дистрибутив в удобное место, запустите сначала `setup.exe`, а затем — `Delta Kinematics Calibration Tool.exe`.

## Исправление ошибок при не-US локали

В OpenDACT есть баг — программа ожидает, что десятичная часть отделяется точкой, а в России и некоторых других странах используется запятая. При встрече “неожиданного” символа софт падает. Автор уведомлён: [https://github.com/RollieRowland/OpenDACT/issues/13](https://github.com/RollieRowland/OpenDACT/issues/13), но пока баг не исправлен, обойти его можно сменой “региональных стандартов” в Windows. Примените эту правку, если софт падает.

![Screenshot](https://cloud.githubusercontent.com/assets/1763243/20276440/4d898040-aaad-11e6-83a2-d61963abfb82.png)

## Сброс настроек EEPROM к значениям по умолчанию перед калибровкой

Сохраните текущие настройки в файл или запишите куда-нибудь на бумаге, чтобы при желании вернуть их назад. Я настоятельно рекомендую сбросить всё к дефолту, чтобы OpenDACT начинал “с чистого листа”.

### Проще всего — через CURA

Запустите CURA и подключитесь к принтеру. В меню выберите `Machine` → `Firmware configuration`. Откроется такое окно:

![CURA EEPROM dialog](https://pp.userapi.com/c836128/v836128745/415b2/MxaS8MbzS88.jpg)

Запишите старые значения на листок, затем введите дефолтные:

| Параметр                           | Описание                              | Значение по умолчанию |
| ---------------------------------- | ------------------------------------- | --------------------- |
| Z max length \[mm]                 | Расстояние от вершины до стола        | Оставить как есть     |
| Tower X endstop offset \[steps]    | Смещение концевика первой башни       | `0`                   |
| Tower Y endstop offset \[steps]    | Смещение концевика второй башни       | `0`                   |
| Tower Z endstop offset \[steps]    | Смещение концевика третьей башни      | `0`                   |
| Diagonal rod length \[mm]          | Длина диагоналей                      | `217`                 |
| Horizontal rod radius at 0,0 \[mm] | Радиус горизонтальных тяг             | `94.5`                |
| Alpha A (210)                      | Угол первой башни (X)                 | `210°`                |
| Alpha B (330)                      | Угол второй башни (Y)                 | `330°`                |
| Alpha C (90)                       | Угол третьей башни (Z)                | `90°`                 |
| Delta Radius A (0)                 | Радиальное смещение первой башни      | `0`                   |
| Delta Radius B (0)                 | Радиальное смещение второй башни      | `0`                   |
| Delta Radius C (0)                 | Радиальное смещение третьей башни     | `0`                   |
| Corr. diagonal A \[mm]             | Исправление наклона первой диагонали  | `0`                   |
| Corr. diagonal B \[mm]             | Исправление наклона второй диагонали  | `0`                   |
| Corr. diagonal C \[mm]             | Исправление наклона третьей диагонали | `0`                   |
| Z-probe height \[mm]               | Высота сенсора Z                      | `0`                   |

### Для продвинутых — смена всех настроек через пакет G-код команд

Скопируйте и выполните этот список в Repetier, Pronterface или другом ПО:

```
M206 T3 P153 X312.000   ; Z max length [mm]
M206 T1 P893 S000       ; Tower X endstop offset [steps]
M206 T1 P895 S000       ; Tower Y endstop offset [steps]
M206 T1 P897 S000       ; Tower Z endstop offset [steps]
M206 T3 P881 X217.000   ; Diagonal rod length [mm]
M206 T3 P885 X95.2      ; Horizontal rod radius at 0,0 [mm]
M206 T3 P901 X210.00    ; Alpha A(210):
M206 T3 P905 X330.00    ; Alpha B(330):
M206 T3 P909 X90.000    ; Alpha C(90):
M206 T3 P913 X0.000     ; Delta Radius A(0):
M206 T3 P917 X0.000     ; Delta Radius B(0):
M206 T3 P921 X0.000     ; Delta Radius C(0):
M206 T3 P933 X0.000     ; Corr. diagonal A [mm]
M206 T3 P937 X0.000     ; Corr. diagonal B [mm]
M206 T3 P941 X0.000     ; Corr. diagonal C [mm]
M206 T3 P808 X0.000     ; Z-probe height [mm]
```

Убедитесь, что параметр “steps per mm” выставлен корректно — при обновлении прошивки он мог сбиться!

## Важная заметка про Z-пробник

Если вы используете стоковый Z-probe — пропустите этот раздел.

Если вы поставили «щёлкающий» зонд, убедитесь, что в EEPROM `Z-probe height [mm]` = 0, а `Z max length [mm]` уменьшено на высоту зонда.

Например, мой принтер 311.82 мм в высоту, а зонд “teddybear” на шарнире имеет высоту 12.4 мм. Вычитаю 12.4 из 311.82 и получаю 299.42 мм — это новое значение `Z max length [mm]`.

![Teddy with boner](https://scontent-ams3-1.xx.fbcdn.net/v/t1.0-9/16195531_10158495767570354_6174518943208334893_n.jpg?oh=798154abea958b18114b8c29e6ea8d4f\&oe=59636BB6)

*(Если вам нравится этот дизайн, можете купить его на [Pinshape](https://pinshape.com/items/31151-3d-printed-z-eddy-the-micromake-z-probe-e3d-v5-fits-afinibot-etc). Стоит своих денег.)*

## Калибровка принтера с OpenDACT

*(Скриншоты сделаны в старой версии программы. У вас будет «3.1.0A», интерфейс слегка отличается.)*

1. Запустите OpenDACT.
2. В поле **Build diameter** задайте диаметр круга для зондирования. Не нужно жадничать — края стекла измерять бессмысленно: конструкция дельты гнётся, и данные будут не точными. Достаточно 100–120 мм.
3. В **Diagonal rod** введите `217`.
4. Выберите порт и Baud rate = `250000`, нажмите **Connect**.
5. Нажмите **Advanced**. Убедитесь, что **Z-minimum type** = `FSR` (а не `Z-probe`), а **FSR plate offset** и **Z-probe height** = 0.
6. **Z-probe start height** определяет, с какой высоты принтер будет медленно опускаться. Слишком высокая — процесс долго идёт, слишком низкая — риск столкновения на полной скорости. Подберите разумно.
7. Нажмите **H.A.I. Calibrate** (не `A.I. Calibrate`) или просто **Calibrate**. Всё остальное сделает программа сама.

![OpenDact - 1st screenshot](https://raw.githubusercontent.com/Bougakov/Micromake-D1-3D-printer/master/opendact1.png)

На второй вкладке увидите, насколько «кривой» ваш принтер:

![OpenDact - 2nd screenshot](https://raw.githubusercontent.com/Bougakov/Micromake-D1-3D-printer/master/opendact2.png)

## Итоговые замечания

OpenDACT нестабилен и иногда глючит, но калибровку делает отлично. Если вы умеете писать на C#, присоединяйтесь к разработке: [https://github.com/RollieRowland/OpenDACT](https://github.com/RollieRowland/OpenDACT) — сообщество будет благодарно.

## Финальный шаг — подстройка Z-высоты

Положите на стекло лист плотной бумаги или обложку журнала. В меню на принтере зайдите в `Configuration` → `Z calib.` → `Z position`.

Медленно вращайте ручку, опуская сопло, пока бумагу не удерживает, но её можно свободно двигать пальцами.

![Bank heist](https://raw.githubusercontent.com/Bougakov/Micromake-D1-3D-printer/master/images/lock%20artist.jpg)

Когда найдёте “идеальный” зазор, выберите `Set Z=0`, чтобы сохранить значение в EEPROM.
