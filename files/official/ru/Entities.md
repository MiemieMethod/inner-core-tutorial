# РАБОТА С СУЩНОСТЯМИ

!> 本文档是为旧的 Inner Core 1.0 或更旧的 Core Engine 编写的。参阅时请仔细分辨，以免造成不必要的麻烦。

Вся работа с мобами осуществляется через модуль `Entity`.

`Entity.getAll()` - возвращает Java-массив всех мобов в мире.

`Entity.getAllJS()` - возвращает JavaScript-массив всех мобов в мире.

`Entity.getExtra(mob, propetry)` - возвращает значение свойства propetry сущности mob в виде строки.

`Entity.putExtra(mob, propetry, value)` - записывает свойство propetry со значением value в виде строки сущности mob.

`Entity.getExtraJson(mob, propetry)` - возвращает свойство propetry сущности mob в виде JS-обьекта.

`Entity.putExtraJson(mob, propetry, value)` - записывает свойство propetry со значением value в виде JS-обьекта сущности mob.

`Entity.addEffect(mob, id, duration, level, [boolean], [boolean])` - ставит сущности mob эффект id продолжительностью duration, уровня level.

`Entity.clearEffect(mob, id)` - удаляет у сущности mob эффект id.

`Entity.clearEffects(mob)` - удаляет у сущности mob все эффекты.

`Entity.damage`Entity(mob, hp)` -  наносит сущности mob урон в hp единиц.

`Entity.heal`Entity(mob, hp)` - лечит сущность mob на hp единиц.

`Entity.getType(a)` - возвращает числовой тип сущности mob.

`Entity.setHitbox(mob, width, height)` - устанавливает сущности mob хитбокс высотой width и шириной height.

`Entity.isExist(mob)` - проверяет, существует ли сущность mob.

`Entity.spawn(id, x, y, z, [extra])` - спавнит существо с типом id, координатами x, y, z и назначает ему extra-свойства.

`Entity.spawnCustom(name, x, y, z, [extra])` - то же, что и `Entity.spawn, но спавнит сущностей со строковым ид name, созданных через MobRegistry.

`Entity.remove(mob)` - удаляет сущность mob из мира.

`Entity.getCustom(mob)` - возвращает прототип сущности mob, если она создана через MobRegistry.

`Entity.getAge(mob)` - возвращает возраст сущности mob.

`Entity.setAge(mob, age)` - устанавливает возраст age сущности mob.

`Entity.getSkin(mob)` - возвращает путь в текстурпаке текстуры сущности mob.

`Entity.setSkin(mob, texture)` - ставит текстуру по пути texture сущности mob.

`Entity.getRender(mob)` - возвращает id рендера сущности mob.

`Entity.setRender(mob, render)` - устанавливает рендером с id render сущности mob.

`Entity.rideAnimal(rider, riding)` - сажает сущность rider на сущность riding.

`Entity.getNameTag(mob)` - возвращает имя сущности mob.

`Entity.setNameTag(mob, name)` - устанавливает сущности mob имя name.

`Entity.getTarget(mob)` - возвращает цель сущности mob.

`Entity.setTarget(attaker, victim)` - ставит сущности attacker цель victim.

`Entity.setMobile(mob, mobile?)` - в зависимости от значения mobile? управляет возможностью сущности mob двигаться.

`Entity.getSneaking(mob)` - проверяет, ползет ли сущность mob.

`Entity.setSneaking(mob, sneak?)` - разрешает или запрещает сущности mob ползти.

`Entity.getRider(mob)` - проверяет, всадник ли сущность mob.

`Entity.getRiding(mob)` - проверяет, сидят ли на сущности mob.

`Entity.setFire(mob, tick)` - заставляет сущность mob гореть в течении tick тиков.

`Entity.getHealth(mob)` - возвращает здоровье сущности mob.

`Entity.setHealth(mob, hp)` - устанавливает здоровье в hp единиц сущности mob.

`Entity.getMaxHealth(mob)` - возвращает максимальное количество здоровья сущности mob.

`Entity.setMaxHealth(mob, maxhp)` - устанавливает максимальное здоровье maxhp сущности mob.

`Entity.setPosition(mob, x, y, z)` - устанавливает сущность mob на координаты x, y, z.

`Entity.getPosition(mob)` - возвращает координаты сущности mob.

`Entity.addPosition(mob, x, y, z)` - относительно перемещает сущность mob на координаты x, y, z.

`Entity.setVelocity(mob, velx, vely, velz)` - устанавливает трехмерное ускорение velx, vely, velz сущности mob.

`Entity.getVelocity(mob)` - возвращает трехмерное ускорение сущности mob.

`Entity.addVelocity(mob, velx, vely, velz)` - добавляет трехмерное ускорение velx, vely, velz сущности mob.

`Entity.getDistanceBetweenCoords(coords1, coords2)` - возвращает расстояние между координатами в обьектах coords1 и coords2.

`Entity.getDistanceToCoords(mob, coords)` - возвращает расстояние от сущности mob до координат в обьекте coords.

`Entity.getDistanceTo`Entity(a, b)` - возвращает расстояние между сущностями mob1 и mob2.

`Entity.getLookAngle(mob)` - возвращает угол обзора сущности mob.

`Entity.setLookAngle(mob, jaw, pitch)` - устанавливает сущности углы обзора jaw и pitch.

`Entity.getLookVector(mob)` - возвращает вектор взгляда сущности mob.

`Entity.getLookAt(mob, x, y, z)` - возвращает угол обзора сущности mob на координаты x, y, z.

`Entity.lookAt(a, b, c, d)` - устанавливает обзор сущности mob на координаты x, y, z.

`Entity.moveToTarget(mob1, mob2, params)` - передвигает сущность mob1 к сущности mob2 с настройками params.

`Entity.moveToAngle(mob, angle, params)` - передвигает сущность mob на угол angle с настройками params.

`Entity.moveToLook(mob, params)` - перемещает сущность mob в направлении взгляда с настройками params.

`Entity.getMovingVector(mob)` - возвращает вектор движения сущности mob.

`Entity.getMovingAngle(mob)` - возвращает угол передвижения сущности mob.

`Entity.findNearest(x, y, z)` - возвращает ближайшую к координатам x, y, z сущность.

`Entity.getAllInRange(coords, type, maxRange)` - возвращает JS-массив сущностей с типом type в сфере с центром coords и радиусом maxRange.

`Entity.getInventory(mob, [object], [object])` - возвращает инвентарь сущности mob.

`Entity.getProjectileItem(mob)` - проверяет, является ли сущность mob снарядом, если это так, возвращает ее данные как предмета.
