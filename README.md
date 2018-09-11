# ROSREESTR TO COORDINATE

Инструмент, позволяющий вычислять координаты участка по его кадастровому номеру.
Данные берутся с сайта публичной кадастровой карты [http://pkk5.rosreestr.ru/](http://pkk5.rosreestr.ru/).

Результат работы скрипта __не соответствует информации в кадастровой выписке__

~~[DEMO](http://geonote.ru/pkk/)~~ (заблокировано)

## Зависимости

* Python 2.7.x
* numpy
* [OpenCV](http://opencv.org/)
* Pillow
* PyQT4 (optional for GUI)

## Установка

[Подробная инструкция для Windows](https://github.com/rendrom/rosreestr2coord/wiki/Instruction)

Получите последнюю версию из репозитория

    git clone https://github.com/rendrom/rosreestr2coord
    cd ./rosreestr2coord
    python setup.py install

    # Установку `python setup.py install` можно не выполнять

Установка через пакетный менеджер

    pip install rosreestr2coord

## Использование

### Из консоли

    rosreestr2coord -c 38:06:144003:4723
    rosreestr2coord -w -l ./cadastral_numbers_list.txt

   или, без установки

    python rosreestr2coord.py -c 38:06:144003:4723

Во время выполнения скрипта, в директории откуда был произведен запуск будут созданы файлы и папки. 
Поэтому рекомендуется создать отдельную папку для работы с этим приложением из консоли. 

Опции:

* -h - справка
* -c - кадастровый номер
* -p - путь для промежуточных файлов
* -o - путь для полученного  geojson файла
* -e - параметр, определяющий точность аппроксимации
* -t - тип площади: Участки 1, ОКС 5, Кварталы 2, Районы 3, Округа 4, Границы 7, ЗОУИТ 10, Тер. зоны 6, Красные линии 13, Лес 12, СРЗУ 15, ОЭЗ 16, ГОК 9
* -l - пакетная загрузка из списка в текстовом файле (тестовый файл -l list_example.txt )
* -w - переводить координаты в WGS84 EPSG:4326
* -a - добавлять атрибуты участка к параметрам GeoJSON файла
* -d - (для режима --code) выводить окно с совмещением картинки и распознанных точек
* -r - не использовать кэширование
* -P - загрузка через прокси

### Программно

    python

```python
from scripts.parser import Area

area = Area("38:06:144003:4723") # дополнительные аргументы coord_out="EPSG:4326", area_type=1, media-path=MEDIA, 
area.to_geojson()
area.to_geojson_poly()
area.get_coord() # [[[area1_xy], [hole1_xy], [hole2_xy]], [[area2_xyl]]]
area.get_attrs()
```

### GUI

После того как установлен PyQT4 выполнить:

    python gui.py

Для разработки (Node.js, npm, webpack):

    cd ./gui/client
    npm i
    npm run build

## Журнал

* 11.09.2018 - Исправление ошибки формирование полигональной геометрии при экспорте в GEOJSON [#8](https://github.com/rendrom/rosreestr2coord/issues/8) by [denny123](https://github.com/denny123).
* 12.03.2018 - Исправление функции завершения выполнения операций в консоли при нажатии на Ctrl+C.
* 05.03.2018 - Добавлена возможность загрузки через прокси [#7](https://github.com/rendrom/rosreestr2coord/issues/5) by [Niakr1s](https://github.com/Niakr1s).
* 09.03.2017 - Добавлена поддержка пользовательского интерфейса с интерактивной картой.
* 17.10.2016 - Увеличена точность вычисления контуров участков
* 14.10.2016 - Обработка участков с несколькими полигонами
* 06.10.2016 - Осуществление экспорта таблиц в формате csv
* 05.10.2016 - Пакетная загрузка участков по списку кадастровых номеров из файла, перевод координат в WGS84
* 03.10.2016 - Добавлена возможность выбора типа площади
* 05.09.2016 - Изменен формат записи координат, добавлена возможность хранить мультиполигональную геометрию. 
* 23.05.2016 - В тестовом режиме работает восстановление полигонов с отверстиями по PNG.
* 21.05.2016 - Были внесены изменения, чтобы вернуть работу с распознаванием точек по PNG. Упала точность, пропала способность рисовать полигоны и выделять отверстия
* 21.05.2016 - На публичных кадастровых картах заблокировали SVG и внесли ещё некоторые изменения в работу сервисов. В связи с этим перестало работать приложение

## TODO

* ~~Увеличить точность для крупных полигонов (округа, кварталы, районы и др.)~~ загрузка изображения участка по тайлам с высоким разрешением
* ~~Различать мультиполигональную геометрию~~ тестируется
* ~~Рисовать полигон~~ в тестовом режима
* ~~В полигоне находить отверстия~~ в тестовом режиме
* ~~Увеличить точность распознавания углов~~ в тестовом режиме
