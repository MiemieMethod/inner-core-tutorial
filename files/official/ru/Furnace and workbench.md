# СОЗДАНИЕ ВЕРСТАКОВ И ПЕЧЕК

!> 本文档是为旧的 Inner Core 1.0 或更旧的 Core Engine 编写的。参阅时请仔细分辨，以免造成不必要的麻烦。

Для того, чтобы придать tile entity функциональность верстака или печки, существует несколько методов в Recipes.

Создадим блок и tile entity.

```js
//Создадим UID blockWorkbench
IDRegistry.genBlockID("blockWorkbench");

//Создаем наш верстак
Block.createBlock("blockWorkbench", [
    {name: "workbench", texture: [["crafting_table", 0]], inCreative: true}
]);

//Регистрируем его как tileEntity
TileEntity.registerPrototype(IDData.block.blockWorkbench, {
    //Функция тика
    tick: function(){
        //Если интерфейс открыт
        if (this.container.isOpened()){
            //Узнаем результат крафта
            var res = Recipes.getRecipeResult(this.container, "workbench");
            //Если он есть
            if (res){
            //Отрисовываем его
                this.container.setSlot("resultSlot", res.id, res.count, res.data);
            }
            else{
                //Иначе рисуем пустой слот
                this.container.setSlot("resultSlot", 0, 0, 0);
            }
        }
    },

    getGuiScreen: function(){
        return workbenchGui;
    }
});
```

И так, мы видим, что наш верстак отображает результат крафта, когда он возможен.

Создадим интерфейс:

```js
//Создаем обычное окно
var workbenchGui = new UI.StandartWindow();
//Задаем его свойства
arcaneWorkbenchGui.setContent({
    standart: {
        header: {
            text: {
                text: "workbench"
            },
        },
        inventory: {
            standart: true
        },
        background: {
            standart: true
        },
        minHeight: 600
    },
    elements: {
        //Создаем слоты
        "slot0": {type: "slot", x: 467, y: 146, size: 60},
        "slot1": {type: "slot", x: 537, y: 146, size: 60},
        "slot2": {type: "slot", x: 607, y: 146, size: 60},
        "slot3": {type: "slot", x: 467, y: 214, size: 60},
        "slot4": {type: "slot", x: 537, y: 214, size: 60},
        "slot5": {type: "slot", x: 607, y: 214, size: 60},
        "slot6": {type: "slot", x: 467, y: 283, size: 60},
        "slot7": {type: "slot", x: 537, y: 283, size: 60},
        "slot8": {type: "slot", x: 607, y: 283, size: 60},
        //Слот для результата
        "resultSlot": {type: "slot", x: 698, y: 212, size: 60, clicker: {
                //При обычном клике
                onClick: function(position, container, tileEntity){
                    //Пробуем провести крафт
                    var result = Recipes.provideRecipe(container, prefix);
                    //Если получилось
                    if (result){
                    //Добавляем результат в инвентарь
                        Player.getInventory().addItem(result.id, result.count, result.data);
                    }
                },
                onLongClick: function(position, container, tileEntity){
                    this.onClick(position, container, tileEntity);
                }
            }
        },
    }
});
```

Для печи так же, только слотов должно быть 3 или 2. 