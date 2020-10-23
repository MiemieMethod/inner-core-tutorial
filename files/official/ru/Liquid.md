# ЖИДКОСТИ И РАБОТА С НИМИ

!> 本文档是为旧的 Inner Core 1.0 或更旧的 Core Engine 编写的。参阅时请仔细分辨，以免造成不必要的麻烦。

Жидкости в Core Engine представлены как некоторый материал, который может храниться в tile entity или наполнять предмет, но при этом не может быть в мире. Т.е. жидкости могут содержаться только в каких нибудь tile entity (например бочка) или предметах (например ведро).

Core Engine позволяет регистрировать новые жидкости, имеющие название и текстуру, предметы содержащие эти жидкости, а так же дает всем tile entity встроенный объект для хранения и работы с жидкостями, аналогичный контейнеру для хранения предметов и отображения слотов.

Стандартные жидкости - молоко, лава и вода и их текстуры уже зарегистрированы, как и предметы, содержащие их, их строковые id:  
water - вода  
milk - молоко  
lava - лава

## РЕГИСТРАЦИЯ

`LiquidRegistry.registerLiquid("строковый id", "название", [массив имен текстур в интерфейсе])` - регистрирует жидкость под данным строковым id и с данным названием и задает ей варианты текстур интерфейса, которые задаются как массив строковых имен текстур. Текстур интерфейса может быть несколько для того, чтобы при использовании их в шкалах различных размеров, Core Engine подбирал лучшее отношение сторон и изменял размер под размер шкалы (жидкость может быть использована другими модами с другим интерфейсом). Если вы хотите задать только одну текстуру для жидоксти, просто напишите `["имя текстуры"`]`.

`LiquidRegistry.isExists("строковый id")` - проверяет существование жидкости с данным строковым id.

`LiquidRegistry.getLiquidName("строковый id")` - отдает имя жидкости по id или null, если такой жидкости нет

## РЕГИСТРАЦИЯ ПРЕДМЕТОВ

`LiquidRegistry.registerItem("строковый id", {id: id пустого предмета, data: data пустого предмета}, {id: id полного предмета, data: data полного предмета})` - регистрирует предмет, содержащий жидкость с данным строковым id в 2 состояниях - пустой и полный. После регистрации предмет будет доступен для методов наполнения и опустошения.

`LiquidRegistry.getEmptyItem(id, data)` - принимает на вход id и дату полного предмета, возвращает объект формата `{id: id пустого предмета, data: data пустого предмета, liquid: строковый id жидкости, которая содержалась в полном предмете}`, если это не предмет, заполненный какой то жидкостью, возвращает `null`

`LiquidRegistry.getItemLiquid(id, data)` - принимает на вход id и дату полного предмета, возвращает строковый id жидкости, которая в нем содержится, если это не предмет, заполненный какой то жидкостью, возвращает `null`

`LiquidRegistry.getFullItem(id, data, "строковый id")` - принимает на вход пустой предмет и строковый id жидкости, возвращает объект формата `{id: id полного предмета, data: data полного предмета}`, если предмет не является предметом, который может содержать жидкость, или такая жидкость не существует, вернет `null`

## ХРАНИЛИЩЕ ЖИДКОСТИ

Объект хранилища жидкости позволяет хранить жидкости и оперировать с ними. Каждый объект tile entity по умолчанию имеет объект хранилища жидкости (`this.liquidStorage`), которые привязаны к нему, так же его можно создать командой new `LiquidRegistry.Storage()` - новый объект хранилища.

Методы объекта хранилища жидкости:  
`updateUiScale("имя элемента ui", "строковый id", контейнер)` - устанавливает для данного контейнера шкалу с данным именем, как шкалу жидкости с данным строковым id, изменяя текстуру на текстуру жидкости и значение на ее объем относительно лимита. Если параметр контейнер не передан, а хранилище привязано к tile entity, то будет использован контейнер этого tile entity.

`hasDataFor("строковый id")` - проверяет, есть ли в нем данные для данной жидкость

`setLimit("строковый id", лимит в ведрах)` - устанавливает конкретной жидкости лимит количества в ведрах, если вместо строкового id передан null, устанавливает данный лимит для всех жидкостей

`getLimit("строковый id")` - возвращает лимит данной жидкости в ведрах

`setAmount("строковый id", объем в ведрах)` - устанавливает объем данной жидкости в ведрах, без проверки лимита

`getAmount("строковый id")` - возвращает кол-во данной жидкости в ведрах

`getRelativeAmount("строковый id")` - возвращает кол-во данной жидкости, деленное на лимит для данной жидкости (используется для шкал интерфейса)

`getLiquidStored("строковый id")` - возвращает строковый id первой жидкости, которая содержится в данном хранилище и кол-во которой больше 0, подходит для  работы с хранилищем, в котором хранится 1 тип жидкости, а так же сочитается с методом `getLiquid`

`isFull("строковый id")` - возвращает true, если кол-во данной жидкости равно ее лимиту (заполнена ли она до предела), если аргумент не передан, проверяет, заполненны ли все жидкости в хранилище до предела.

`isEmpty("строковый id")` - возвращает true, если кол-во данной жидкости равно 0 (пустота), если аргумент не передан, проверяет, все ли жидкости в хранилище на 0.

`addLiquid("строковый id", объем в ведрах)` - добавляет данный объем данной жидкости в хранилище, возвращает объем лишней жидкости (которая не уместилась в лимит)

`getLiquid("строковый id", объем в ведрах)` - забирает данный объем данной жидкости из хранилища, возвращает объем, который удалось забрать (если жидкости меньше требуемого кол-ва, то отдает не переданный объем, а меньше). Если жидкости в хранилище не хватает и хранилище привязано к tile entity будет вызвано событие requireMoreLiquid в tile entity.