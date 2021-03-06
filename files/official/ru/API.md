# API

!> 本文档是为旧的 Inner Core 1.0 或更旧的 Core Engine 编写的。参阅时请仔细分辨，以免造成不必要的麻烦。

## ОБМЕН API МОДОВ

Core Engine имеет модуль для обмена объектами API между различными модами - `ModAPI`, обмен API осуществляется с помощью методов регистрации и публикации API и с помощью методов его получения. 

Чтобы сделать объект API доступным для других модов используется метод `ModAPI.registerAPI("имя API", объект API)` - после вызова этого метода, остальные моды смогут получить объект API по данному имени.

Чтобы получить объект API нужно добавить специальное событие на имя объекта API, который требуется получить. Событие используется потому, что моды загружаются в произвольном порядке и неизвестно, зарегистрирован ли данный API на момент загрузки мода, который в нем нуждается, даже если мод, который регистрирует, присутсвует. 
Для этого используется метод ModAPI.

```js
addAPICallback("имя API", function(api){
    // api является объектом api, зарегистрированным под данным именем
})
```

Это событие произойдет 1 раз после добавления, при этом если API уже зарегистрирован, то событие произойдет сразу после добавления, иначе оно будет ожидать регистрации.
В этом событии вы можете произвести какие-то действия, связанные с работой с данным API, например при получении `ThaumAPI (api таумкрафта)`, добавить предметам вашего мода аспекты. Если событие не произойдет, значит таумкрафт не установлен и аспекты регистрировать не надо.

ИСПОЛЬЗОВАНИЕ СОБЫТИЙ В СКРИПТЕ-ЛАУНЧЕРЕ
Если мод не может работать без какого-то объекта API (например это дополнение к какому-то моду), то разумно будет сделать так, что мод запускается по событию регистрации данного API и сразу передать ему объект API.

К примеру вот код лаунчера мода, который является дополнением к таумкрафту и которому обязателен `ThaumAPI`:

```js
ModAPI.addAPICallback("ThaumAPI", function(api){
    // по событию регистрации ThaumAPI запустить основной мод
    Launch({
        ThaumAPI: api // в качестве дополнительного значения при запуске под именем ThaumAPI передать полученный объект api
    });
});
```

## ДРУГИЕ МЕТОДЫ ModAPI

`ModAPI.requireAPI("имя API")` - отдает объект API по данному имени, если он зарегистрирован. Несовершенен, потому что порядок загрузки неизвестен. Лучше используйте ModAPI.addAPICallback.

`ModAPI.getModByName("имя мода")` - отдает объект мода, отвечающий за его файлы и загрузку, если мод загружен. Несовершенен, потому что порядок загрузки неизвестен. Лучше используйте ModAPI.addModCallback.


`ModAPI.addModCallback("имя мода", function(mod){/* mod является объектом мода */})` - добавляет событие, которое произойдет при загрузке мода под данным именем, которому будет передан объект мода. Аналогичен по действию ModAPI.addAPICallback, только для модов.

`ModAPI.isModLoaded("имя мода")` - проверяет, загружен ли мод с этим именем. Актуальна в в событиях PostLoaded и ModsLoaded, которые происходят после загрузки всех модов.

`ModAPI.requireGlobal("имя переменной или исполняемая строка")` - возвращает переменную с данным именем из самого Core Engine, если передана исполняемая строка, выполняет ее в пространстве имен Core Engine и возвращает результат, при возникновении ошибки возвращает null и пишет ошибку в лог. Может быть использована для получения скрытых модулей или значений ModPE (они могут работать некорректно).
