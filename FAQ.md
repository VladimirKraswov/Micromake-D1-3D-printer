# FAQ по 3D-принтеру Micromake D1

Этот FAQ написан для [группы владельцев этого недорогого и отличного 3D-принтера на Facebook](https://www.facebook.com/groups/173676226330714/). К сожалению, Facebook неудобен для публикации больших текстов, поэтому я размещаю их здесь. Если хотите что-то добавить — присылайте pull request.

<!-- [Старая русскоязычная группа поддержки в ВК](https://vk.com/micromake3d) (её создали китайские сотрудники магазина, но забросили) и [новая, которую ведут энтузиасты](https://vk.com/micromake_d1). -->

### Варианты Micromake D1 и его «клонов»

Micromake D1 поставляется в [трёх версиях](http://ali.ski/vzso1??&h=7AQHgTTr_&s=1):

* Самая дешевая «шкивовая» версия (с роликами на каретках)
* Более дорогая версия с линейными рельсами (с подшипниками)
* Самая дорогая «hiwin» — на высококачественных направляющих HIWIN

Опционально принтер может быть укомплектован подогреваемым столом (для печати ABS), вместе с ним идёт более мощный блок питания 16,5 А вместо штатного слабого.

Также встречаются два варианта главной платы: старая и новая.

### Что учесть перед покупкой

* Можно получить кешбэк 5 % при покупке на AliExpress через кэшбэк-сервисы (Ebates и др.).
* Обсуждения аксессуаров и модификаций:

  * [https://www.facebook.com/groups/173676226330714/permalink/333773956987606/](https://www.facebook.com/groups/173676226330714/permalink/333773956987606/)
  * [https://www.facebook.com/groups/173676226330714/permalink/333812513650417/](https://www.facebook.com/groups/173676226330714/permalink/333812513650417/)

### Проверенные продавцы

* [Официальный магазин на AliExpress](https://www.aliexpress.com/store/product/Micromake-3D-printer-pulley-version-DIY-kit-Metal-Printer-3d-Printer-Kossel-Delta-3d-Printer-Kit/2128317_32647812923.html) — оригинальная версия с чёрными анодированными алюминиевыми профилями и литыми пластиковыми деталями.

### Обзоры и отзывы покупателей

**Статьи на русском:**

* Обзор версии HiWin: [http://mysku.ru/blog/aliexpress/41006.html](http://mysku.ru/blog/aliexpress/41006.html)
* Обзор D1 с TaoBao: [http://mysku.ru/blog/aliexpress/39495.html](http://mysku.ru/blog/aliexpress/39495.html)
* Обзор апгрейда подогреваемого стола: [http://mysku.ru/blog/aliexpress/43168.html](http://mysku.ru/blog/aliexpress/43168.html)

**Видео на YouTube:**

* (tbd)

### Руководства по сборке

* PDF-мануал (англ. и рус.) — ссылка в разделе «Официальное ПО» ниже.
* Официальный YouTube-канал (англ./кит.) — [https://www.youtube.com/channel/UCY9sDyEi81z3GjXhwGiP\_RA/playlists](https://www.youtube.com/channel/UCY9sDyEi81z3GjXhwGiP_RA/playlists)
* Плейлисты на Youku (кит.): [http://www.youku.com/playlist\_show/id\_23218776.html](http://www.youku.com/playlist_show/id_23218776.html)

### Типичные ошибки при сборке и советы

* На экструдере два вентилятора: один постоянно обдувает алюминиевые ребра радиатора, второй включается периодически для охлаждения печати. Проверьте, что первый вентилятор всегда включён при работе принтера.
* Для улучшения теплопередачи разберите хотэнд, нанесите термопасту на стык медной насадки и радиаторного блока. Обмотайте фитинг, идущий в квадратный нагревательный блок, сантехнической фум-лентой — это снизит теплопроводность в холодной зоне.

### Официальное ПО и прошивки

Сборочные видео и ПО на Google Drive: [https://drive.google.com/drive/folders/0B1DQUrzkDP-tNDU0NXhVcGhlc0k?usp=sharing](https://drive.google.com/drive/folders/0B1DQUrzkDP-tNDU0NXhVcGhlc0k?usp=sharing)

Актуальные версии (на момент написания):

* Repetier-Firmware 0.92.9 и Micromake 2.0.2
* CURA 15.04.0917 (китайская сборка, переключение на англ. в настройках)

Другие репозитории:

* GitHub: [https://github.com/coldnew/MICROMAKE](https://github.com/coldnew/MICROMAKE) (неизвестно, свежая ли)

> После установки подогреваемого стола не забудьте в CURA отметить галочку "Heated bed" и прошить принтер новой прошивкой.

### Автовыравнивание и калибровка

**[Руководство по калибровке](./calibrating.md)**

* Что такое «горизонтальный радиус», «длина диагонали» и т. д.: [https://forum.repetier.com/discussion/435/initial-delta-value-setup](https://forum.repetier.com/discussion/435/initial-delta-value-setup)
* Длина тяг (hole-to-hole) для версии с роликами — 215–217 мм (в CURA по умолчанию ошибочно 210 мм).
* Горизонтальный радиус \~95 мм.
* Онлайн-калькулятор: [http://escher3d.com/pages/wizards/wizarddelta.php](http://escher3d.com/pages/wizards/wizarddelta.php)
* Видео калибровки:

  * Repetier (кит.): [https://www.youtube.com/watch?v=UFPXBdeJmg0](https://www.youtube.com/watch?v=UFPXBdeJmg0)
  * CURA (англ.): [https://www.youtube.com/watch?v=TFeyNqYMFgU](https://www.youtube.com/watch?v=TFeyNqYMFgU)
  * Z-Leveling (Repetier-FW): [https://www.youtube.com/watch?v=L9URtv2LqKc](https://www.youtube.com/watch?v=L9URtv2LqKc)
* OpenDACT: [http://forum.seemecnc.com/viewtopic.php?f=36\&t=8698](http://forum.seemecnc.com/viewtopic.php?f=36&t=8698)

### Типичные проблемы при печати и их решения

* Англ.: [http://reprap.org/wiki/Print\_Troubleshooting\_Pictorial\_Guide](http://reprap.org/wiki/Print_Troubleshooting_Pictorial_Guide)
* Рус.: [http://3dtoday.ru/blogs/leoluch/defects-3d-printing-will-try-to-introduce-a-classification/](http://3dtoday.ru/blogs/leoluch/defects-3d-printing-will-try-to-introduce-a-classification/)

### Апгрейды и модификации

**Каркас:**

* Замена квадратных гаек на T‑гайки.

**Хотэнд:**

* Стандартный E3D V5 с медным соплом 0.4 мм и тефлоновой трубкой.
* Рекомендуется иметь набор сверл 0.2–0.4 мм для прочистки сопла.

**Ремни и шкивы:**

* GT2 ремни.
* Замена шкивов на зубчатые.

**Тяги:**

* Сток 217 мм.
* Замена на тяги Traxxas, установка пружин.

**Подогреваемый стол:**

* Изоляция под столом (сил. коврик или пробка) повышает скорость прогрева.
* Повышение напряжения блока питания с 12 В до 13.9 В ускоряет разогрев (безопасно).

**Блок питания и питание платы:**

* Заводской зелёный разъём не рассчитан на 16 А — пожароопасен! Лучше припаять толстый провод к плате на предназначенные для этого площадки.
* Альтернатива: разъём от RC моделей.
* Вынос блока питания под корпус (мод в блоге).

**Эффектор:**

* Магнитный шарнир.
* Подсветка светодиодами.

**Позиционирование катушки:**

* Держатель катушки и установка экструдерной головы.

**Замена датчика Z:**

* FSR (датчики давления).
* Весовой датчик (load cell).
* Механический датчик.
* Микровыключатель.

**Шаговые двигатели:**

* Включение 1/32 микрошагов на старой плате.

**Замена платы на 32‑бит:**

* Duet.
* MKS SBASE.

**Удалённое управление:**

* OctoPrint на Orange Pi PC.

### Альтернативное ПО управления и слайсеры

* Прошивки от @zzcatvs в группе.
* Список слайсеров: [https://www.facebook.com/groups/173676226330714/permalink/320261655005503/](https://www.facebook.com/groups/173676226330714/permalink/320261655005503/)
* Настройки для RepetierHost: [https://www.facebook.com/groups/173676226330714/permalink/323009244730744/](https://www.facebook.com/groups/173676226330714/permalink/323009244730744/)
* Альтернативная прошивка RichCal с автолевелом.
* Marlin.
* Simplify3D.
* KISSlicer.
* RepetierHost + калибровка (видео).

### Другие ресурсы

* Thingiverse: [http://www.thingiverse.com/groups/micromake/page:1](http://www.thingiverse.com/groups/micromake/page:1)
* Reddit: [https://www.reddit.com/r/3Dprinting/comments/4vrmsh/micromake\_d1\_or\_alternative/](https://www.reddit.com/r/3Dprinting/comments/4vrmsh/micromake_d1_or_alternative/)
* RepRap Wiki (семейство Kossel): [http://reprap.org/wiki/Kossel](http://reprap.org/wiki/Kossel)
