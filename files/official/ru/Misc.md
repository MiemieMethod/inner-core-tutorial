# РАЗНОЕ

!> 本文档是为旧的 Inner Core 1.0 或更旧的 Core Engine 编写的。参阅时请仔细分辨，以免造成不必要的麻烦。

## ИГРА

Для взаимодействия с игрой в общем используется модуль `Game.

`Game.prevent()` - отменяет поведения игры в контексте.

`Game.message(msg)` - пишет сообщение msg в чат.

`Game.tipMessage(msg)` - пишет сообщение msg над инвентарем.

`Game.dialogMessage(message, title)` - пишет сообщение msg в диалог с заголовком title.

`Game.setDifficulty(difficulty)` - устанавливает сложность difficulty.

`Game.getDifficulty()` - возвращает сложность.

`Game.getMinecraftVersion()` - возвращает версию игры.

`Game.getEngineVersion()` - возвращает версию движка.

[`Game.selectLevel(name)`] - запускает уровень level.

`Game.setCamera(entity)` - переключает камеру игрока на существо entity.

`Game.setTickingSpeed(speed)` - устанавливает скорость игры в speed тиков в секунду.

## ЛОГ

Для взаимодействия с логом существует модуль `Logger.

(i) Если log не указан, то используется основной.

`Logger.wipe(log)` - стирает лог с именем log.

`Logger.buffer - текущий лог.

`Logger.message(log, message)` - пишет в лог log сообщение message.

`Logger.flush(log)` - пишет текущий лог в log.

`Logger.formatLog(log)` - форматирует лог log.

`Logger.showAsDialog(title, paragraf)` - выводит текущий лог в окно с именем title.

`Logger.Log(msg, prefix, log)` - записывает сообщение msg с префиксом prefix в лог log.

`Logger.LogError(error, log)` - записывает ошибку error в лог log.

## ЧАСТИЦЫ

`Particles.addParticle(x, y, z, id, vx, vy, vz, data)` - спавнит частицу на координатах x, y, z, с ID id, трехмерным ускорением vx, vy, vz, с метаданными data.

`Particles.line(particle, coords1, coords2, gap, vel, data)` - спавнит линию частиц particle от точки coords1 до точки coords2 с промежутком gap, ускорением vel и метаданными data.

## ПЕРЕВОД

`Translation.addTranslation(original, lang)` - создает перевод lang для строки original.

Формат обьекта lang:

```js
{
    //префикс языка:переведенная строка
}
```

`Translation.getLanguage()` - возвращает текущий язык игры.

`Translation.translate(original)` - переводит строку, если имеется перевод на текущий язык.

`Translation.translateItem(name)` - переводит название предмета name.

`Translation.translateAllItems()` - переводит названия всех предметов.

## ОТЛАДКА

`Debug.addParticle(x, y, z, id, vx, vy, vz, data)` - создает отладочные частицы, аргументы аналогичны `Particles.addParticle`.

`Debug.message(message)` - пишет отладочное сообщение message в чат.

`Debug.warning(message)` - пишет тревожное сообщение message в чат.

`Debug.error(message)` - пишет сообщение об ошибке message в чат.

## МНОГОПОТОЧНОСТЬ

Для управления потоками существует модуль `Threading`.

`Threading.initThread(name, func, priority)` - создает поток с именем name, выполняющий функцию name с приоритетом priority.

`Threading.getThread(name)` - возвращает поток с именем name.
