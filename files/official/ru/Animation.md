# АНИМАЦИИ

!> 本文档是为旧的 Inner Core 1.0 或更旧的 Core Engine 编写的。参阅时请仔细分辨，以免造成不必要的麻烦。

В Core Engine возможно создавать анимации текста, моделей, предметов в мире.

## БАЗОВАЯ АНИМАЦИЯ

Рассмотрим базовый тип анимации.

Создать ее можно так:

```js
var ani = new Animation.Base(x, y, z);
//x, y, z - координаты
```

Однако просто анимация без настроек просто бессмыслена. Да и менять анимацию потом очень проблематично.

Создадим обновляемый обьект и анимацию внутри него.

```js
//Создаем обьект
var ani_upd = {
    //Задаем его координаты
    x: 0,
    y: 0,
    z: 0,
    //Обьект анимации
    ani: {},
    //Флажок инициализации
    init: false,
    //Функция инициализации анимации
    init_animation: function(){
        //Создаем анимацию
        this.ani = new Animation.Base(this.x, this.y, this.z);

    },
    //Функция обновления анимации
    update_animation: function(){
        //Устанавливаем позицию анимации
        this.ani.setPos(this.x, this.y, this.z);
    },
    //Обновление
    update: function(){
        //Если анимация не инициализирована
        if (!this.init){
            //Инициализировать
            this.init_animation();
            //Поднять флажок инициализации
            this.init = true;
        }
        //Иначе
        else{
            //Обновляем анимацию
            this.update_animation();
        }
        //Обновляем позицию анимации
        this.x = Player.getPosition().x;
        this.y = Player.getPosition().y+4;
        this.z = Player.getPosition().z;
    }
};

//При запуске в мир запускаем анимацию
Callback.addCallback("LevelLoaded", function(){
    Updatable.addUpdatable(ani_upd)
});
```

Но в таком случае над вашей головой будет висеть кролик.
Чтобы настроить анимацию, нужно применить ее метод describe.

```js
{
…
init_animation: function(){
        //Создаем анимацию
        this.ani = new Animation.Base(this.x, this.y, this.z);
        //Настраиваем ее
        this.ani.describe({
            //Здесь могут находиться настройки анимации
            //Можно установить существующий рендер
            // (i) Перед тем, как написать этот код, запишите где-нибудь: var NativeEntityRenderType = ModAPI.requireGlobal("EntityRenderType"); Это позволит использовать рендеры из ModPE
            render: NativeEntityRenderType.pig,
            //А можно установить и свой собственный
            renderAPI: ваш обьект рендера,
            //Текстура анимации
            skin: "/mob/pig.png",
            //Остальные свойства ненужны
        });
        //Загружаем анимацию в мир
        this.ani.load();
    }
…
}
```

Теперь мы можем использовать нашу анимацию и отображать ее в мире.

## ПРЕДМЕТЫ

Также можно создать изображение в мире предмета.

```js
var ani_item = new Animation.Item(x, y, z);
```

В остальном эта анимация похожа на обычную, только настраивается она методом animation.describeItem

```js
ani_item.describeItem({
    //Настраиваем характеристики отоюражаемого предмета
    id: 280,
    count: 1,
    data: 0,
    //Рендерим палку
    //Устанавливаем размер
    size: 1,
    //Устанавливаем выравнивание по оси x/y/z
    rotation: "x\y\z",
    //Убираем неровности, если отрисовывается несколько анимаций
    notRandomize: true
});
```

## ТЕКСТ

Также в мире можно отображать в мире текст:

```js
var ani_txt = new Animation.Text(x, y, z);
//Настроим отображаемый текст
ani_txt.setText(text);
```
