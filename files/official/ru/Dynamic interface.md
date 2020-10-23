# ДИНАМИЧНЫЙ ИНТЕРФЕЙС

!> 本文档是为旧的 Inner Core 1.0 或更旧的 Core Engine 编写的。参阅时请仔细分辨，以免造成不必要的麻烦。

Как упоминалось в уроках ранее, объект описания интерфейса можно изменять, пока интерфейс открыт и изменения тут же будут отображаться в интерфейсе. Больше всего это подходит для элементов, которые должны появляться/изменяться/исчезать только в каких-то определенных условиях (например кнопка, которая появляется, только если в слоте есть предмет)

## ПОЛУЧЕНИЕ ОБЪЕКТА ОПИСАНИЯ

По скольку интерфейс всегда открывается для контейнера, то контейнер имеет метод получения объекта описания интерфейса для динамичного изменения. Этот метод - `getGuiContent()` - упоминался в прошлой главе и теперь будет разобран подробнее.

Формат объекта описания можно посмотреть в предыущих главах, там же говорится, что динамичными (т.е. изменения которых тут же отобразятся в интерфейсе), являются только части, отвечающие за фон и за элементы - `drawing` и `elements`, но не параметры самого интерфейса (заголовок, размер и т.п)

И так, предположим мы получили объект описания:

```js
var content = container.getGuiContent();
```

Теперь, чтобы изменить в нем фон, мы можем просто присвоить drawing новое значение:

```js
content.drawing = [
    {type: "background", color: android.graphics.Color.RED}
] // мы удалили все прошлые команды и теперь рисуем просто красный фон
```

С элементами все несколько иначе, чтобы что-то сделать с элементом, нужно обратиться к нему по его имени: `content.elements["имя элемента"]`, после чего с ним можно сделать 3 разных действия:

1. Приравнять новому элементу - если элемент с данным именем уже существовал, то старый будет полностью заменен на новый, если элемента не было, то будет просто создан новый элемент

2. Изменить какое-то значение - элемент  с данным именем будет изменен потому что какой то его параметр был изменен.

3. Приравнять null - элемент с данным именем будет удален

ПРИМЕР #1: (изменение параметров)

```js
// код ниже при старте откроет интерфейс с 1 кнопкой, при нажатии на которую она будет перемещаться вниз
var testUIScreen = new UI.StandartWindow({
    standart: {
        header: {
            text: {
                text: "TEST UI"
            }
        },
        inventory: {
            standart: true
        },
        background: {
            standart: true
        }
    },
    drawing: [],
    elements: {
        "test_button": {type: "button", bitmap: "close_button_up", x: 400, y: 100, scale: 8, clicker: { // самая обычная кнопка с именем test_button
                onClick: function(container){ // функция короткого нажатия, все параметры кроме первого опущены за ненадобностью
                    var content = container.getGuiContent(); // получаем объект описания, по скольку кнопку можно нажать только с открытым интерфейсом, то он точно определен
                    content.elements["test_button"].y += 10; // немного сдвигаем кнопку вниз, добавляя 10 к координате y
                }
            }
        }
    }
});
// открыть интерфейс
UI.testUI(testUIScreen);
```

ПРИМЕР #2: (удаление и создание)

```js
// после нажатия на кнопку, удаляет ее и создает текст о том, что она удалена
var testUIScreen = new UI.StandartWindow({
    standart: {
        header: {
            text: {
                text: "TEST UI"
            }
        },
        inventory: {
            standart: true
        },
        background: {
            standart: true
        }
    },
    drawing: [],
    elements: {
        "test_button": {type: "button", bitmap: "close_button_up", x: 400, y: 100, scale: 8, clicker: { // самая обычная кнопка с именем test_button
                onClick: function(container){ // функция короткого нажатия, все параметры кроме первого опущены за ненадобностью
                    var content = container.getGuiContent(); // получаем объект описания, по скольку кнопку можно нажать только с открытым интерфейсом, то он точно определен
                    content.elements["test_button"] = null; // удаляем кнопку
                    content.elements["new_element"] = {type: "text", text: "button removed", x: 400, y: 50, width: 200, height: 50}; // создаем текст о том, что она удалена
                }
            }
        }
    }
});
// открыть интерфейс
UI.testUI(testUIScreen);
```

ПРИМЕР #3: (полная замена + изменение фона)

```js
// после нажатия на кнопку, удаляет ее и создает текст о том, что она удалена, а так же делает фон красным
var testUIScreen = new UI.StandartWindow({
    standart: {
        header: {
            text: {
                text: "TEST UI"
            }
        },
        inventory: {
            standart: true
        },
        background: {
            standart: true
        }
    },
    drawing: [],
    elements: {
        "test_button": {type: "button", bitmap: "close_button_up", x: 400, y: 100, scale: 8, clicker: { // самая обычная кнопка с именем test_button
                onClick: function(container){ // функция короткого нажатия, все параметры кроме первого опущены за ненадобностью
                    var content = container.getGuiContent(); // получаем объект описания, по скольку кнопку можно нажать только с открытым интерфейсом, то он точно определен
                    content.elements["test_button"] = {type: "text", text: "button removed", x: 400, y: 50, width: 200, height: 50}; // заменяем элемент кнопки на текст о том, что она удалена
                    content.drawing = [
                        {type: "background", color: android.graphics.Color.RED}
                    ]; // мы удалили все прошлые команды и теперь рисуем просто красный фон
                }
            }
        }
    }
});
// открыть интерфейс
UI.testUI(testUIScreen);
```

ПРИМЕР #4: (tile entity):

```js
// добавляет tile entity интерфейс с кнопкой, которая медленно опускается, пока интерфейс открыт
var testUIScreen = new UI.StandartWindow({
    standart: {
        header: {
            text: {
                text: "TEST UI"
            }
        },
        inventory: {
            standart: true
        },
        background: {
            standart: true
        }
    },
    drawing: [],
    elements: {
        "test_button": {type: "button", bitmap: "close_button_up", x: 400, y: 100, scale: 8, clicker: { // самая обычная кнопка с именем test_button
                onClick: function(container){/* ничего не делаем при клике */}
            }
        }
    }
});
```

Прототип tile entity:

```js
{
    // ...
    getGuiScreen: function(){ // функция получения интерфейса - отдаем наш интерфейс
        return testUIScreen;
    },

    tick: function(){
        var content = this.container.getGuiContent(); // пытаемся получить объект описания
        if (content){ // если получить удалось
            content.elements["test_button"].y += .3; // медленно сдвигаем кнопку вниз
        }
    }
    // ...
}
```
