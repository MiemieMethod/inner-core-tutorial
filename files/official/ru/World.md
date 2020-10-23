# МОДУЛЬ РАБОТЫ С МИРОМ

!> 本文档是为旧的 Inner Core 1.0 或更旧的 Core Engine 编写的。参阅时请仔细分辨，以免造成不必要的麻烦。

Вся работа с миром осуществляется через модуль `World`.

## РАБОТА С БЛОКАМИ

`World.setBlock(x, y, z, id, metadata)` - устанавливает блок с данным ID и metadata на координаты x, y, z

`World.setFullBlock(x, y, z, fullTile)` - принимает объект со значениями id, data, устанавливает блок с данным ID и metadata на координаты x, y, z

`World.getBlock(x, y, z)` - возвращает объект со значениями id, data, которые передают ID и metadata на координатах x, y, z

`World.getBlockID(x, y, z)` - возвращает объект со значениями id, data, которые передают ID и metadata блока на координатах x, y, z

`World.getBlockID(x, y, z)` - возвращает id блока на координатах x, y, z

`World.getBlockData(x, y, z)` - возвращает metadata блока на координатах x, y, z

`World.destroyBlock(x, y, z, drop)` - уничтожает блок на координатах x, y, z, если drop истенен, то дропает его

## РАБОТА С TILE ENTITY И КОНТЕЙНЕРАМИ

`World.getTileEntity(x, y, z)` - возвращает объект tile entity на координатах x, y, z, или null, если его нет

`World.addTileEntity(x, y, z)` - если блок на координатах x, y, z является блоком tile entity и не активирован, то активирует его и возвращает созданный объект tile entity

`World.removeTileEntity(x, y, z)` - если на координатах x, y, z есть tile entity, уничтожает его.

`World.getConteiner(x, y, z)` - если на координатах x, y, z находится сундук, печь или tile entity, возвращает его контейнер, если это печь или сундук, то контейнер обладает методами `refreshSlots()`/`applyChanges()`

## РАБОТА С ОКРУЖЕНИЕМ

`World.getThreadTime()` - возвращает номер такта основного потока со входа в мир

`World.get`WorldTime()` - возвращает игровое время в тиках

`World.set`WorldTime()` - устанавливает игровое время в тиках

`World.setDayMode(isDay)` - заставляет солнце передвигаться в дневную позицию, если isDay истинно, иначе в ночную

`World.setNightMode(isNight)` - заставляет солнце передвигаться в ночную позицию, если isNight истинно, иначе в дневную

`World.getWeather()` - возвращает объект с данными о погоде, rain - уровень дождя, thunder - уровень грозы

`World.setWeather(weather)` - устанавливает погоду по объекту с данными о погоде, rain - уровень дождя, thunder - уровень грозы

## ДРУГИЕ МЕТОДЫ

`World.drop(x, y, z, id, count, data)` - дропает предмет [id, count, data] на координаты x, y, z и возвращает его

`World.explode(x, y, z, power, fire)` - создает взрыв на координатах x, y, z мощности power, если fire истинно, поджигает все в области взрыва

`World.getBiome(x, z)` - отдает числовой ID биома на координатах x, z

`World.getBiomeName(x, z)` - отдает название биома на координатах x, z

`World.getGrassColor(x, z)` - отдает цвет травы на координатах x, z ввиде одного числа

`World.setGrassColor(x, z, color)` - устанавливает цвет травы на координатах x, z ввиде одного числа

`World.getGrassColorRGB(x, z)` - отдает цвет травы на координатах x, z ввиде объекта, содержащего три компоненты цвета r, g, b, каждая от 0 до 255

`World.setGrassColorRGB(x, z, rgb)` - устанавливает цвет травы на координатах x, z ввиде объекта, содержащего три компоненты цвета r, g, b, каждая от 0 до 255

`World.canSeeSky(x, y, z)` - отдает true, если из точки x, y, z видно небо, иначе false

`World.playSound(x, y, z, "name", volume)` - проигрывает стандартный звук "name" на координатах x, y, z с громкостью volume от 0 до 100

`World.playSoundAtEntity(entity, "name", volume)` - проигрывает стандартный звук "name" на координатах данного моба entity с громкостью volume от 0 до 100
