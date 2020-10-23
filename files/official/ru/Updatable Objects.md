# ОБНОВЛЯЕМЫЕ ОБЬЕКТЫ

!> 本文档是为旧的 Inner Core 1.0 或更旧的 Core Engine 编写的。参阅时请仔细分辨，以免造成不必要的麻烦。

Модуль обновляемых обьектов, или Updatable - одна из важнейших технологий в CoreEngine наряду с событиями. На них построены генерация, tile entity, мобы и все остальное.

Для того, чтобы создать обновляемый обьект, используйте метод:

```js
Updatable.addUpdatable(updatable)
```

Обновляемый обьект должен иметь метод update и желательно условие самоуничтожения.

С помощью управляемых обьектов можно, например, создавать обработчики некоторых событий, не создавая для этого моба или блок.

Важно! Для модификации извне сохраните updatable в переменную, затем укажите ее в создании обновляемого обьекта как аргумент.

Если нужно принудительно уничтожить обьект, используйте:
`Updatable.removeUpdatable(updatable)`.

`Updatable.isInUpdate(updatable)` - проверяет, обновляется ли updatable.

## ПРИМЕР

Пример кода на свинопад с использованием Updatable:

```js
var SubRandomHandler = {
    pigfallChance: function(){
        return Math.round(Math.random() * 25))
    },
    update: function(){
        if (this.pigfallChance() == 0){
            Updatable.addUpdatable(PigfallHandler);
            this.remove = true;
        }
    }
};

var PigfallHandler = {
    pigPower: 40,
    pigfallTime: 6000,
    pigDiapason: 200,
    checkSecondPigPosition: function(){
        return {
            x: Math.round(Math.random() * this.pigDiapason) + Player.getPosition().x,
            y: 80 + Math.round(Math.random() * 46),
            z: Math.round(Math.random() * this.pigDiapason + Player.getPosition().z
        };
    },
    spawnPig: function(){
        let pos = this.checkSecondPigPosition();
        Entity.spawn(12, pos.x, pos.y, pos.z);
    },
    pigChance: function(){
        if (Math.round(Math.random() * this.pigPower + 1) == 0){
            return true;
        }
    },
    update: function(){
        if (this.pigChance){
            this.spawnPig();
            this.pigfallTime--;
            this.pigDiapason = this.pigfallTime / this.pigPower;
            if (this.pigfallTime % 120){
                this.pigPower--;
            };
        };
        if (this.pigfallTime <= 0){
            Updatable.addUpdatable(SubRandomHandler);
            this.remove = true;
        };
    }
};

Callback.addCallback("LevelLoaded",function(){
    Updatable.addUpdatable(SubRandomHandler)
});
```
