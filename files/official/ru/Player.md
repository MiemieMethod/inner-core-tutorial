# ИГРОК

!> 本文档是为旧的 Inner Core 1.0 或更旧的 Core Engine 编写的。参阅时请仔细分辨，以免造成不必要的麻烦。

Для взаимодействия с игроком существует модуль `Player.

`Player.get()` - возвращает игрока как сущность для работы с ним с помощью модуля Entity.

`Player.getNameForEnt(Player)` - возвращает имя игрока как сущности `Player.

`Player.getName()` - возвращает имя игрока.

`Player.getDimension()` - возвращает id измерения, в котором находится игрок.

`Player.isPlayer(mob)` - проверяет, является ли сущность mob игроком.

`Player.getPointed()` - возвращает обьект, на который смотрит игрок.

`Player.getInventory(mob, [object], [object])` - возвращает инвентарь игрока как сущности mob.

`Player.getCarriedItem()` - возвращает предмет, который держит игрок.

`Player.setCarriedItem(id, count, data)` - заставляет игрока держать предмет с id, count, data.

`Player.decreaseCarriedItem(count)` - удаляет count предметов из руки игрока.

`Player.setPosition(x, y, z)` - ставит игрока на координаты x, y, z.

`Player.getPosition()` - возвращает координаты игрока.

`Player.addPosition(x, y, z)` - смещает игрока на координаты x, y, z.

`Player.setVelocity(velx, vely, velz)` - ставит игроку трехмерное ускорение velx, vely, velz.

`Player.getVelocity()` - возвращает трехмерное ускорение игрока.

`Player.addVelocity(velx, vely, velz)` - добавляет игроку трехмерное ускорение velx, vely, velz.

`Player.experience()` - возвращает состояние опыта игрока.

`Player.getExperience()` - возвращает опыт игрока.

`Player.setExperience(exp)` - устанавливает exp опыта игроку.

`Player.addExperience(exp)` - добавляет exp опыта игроку.

`Player.level()` - возвращает состояние уровня игрока.

`Player.getLevel()` - возвращает уровень игрока.

`Player.setLevel(lvl)` - ставит lvl уровень игроку.

`Player.addLevel(lvl)` - добавляет lvl уровней игроку.

`Player.flying()` - возвращает состояние полета игрока.

`Player.getFlyingEnabled()` - проверяет, может ли летать игрок.

`Player.setFlyingEnabled(fly?)` - устанавливает возможность игрока летать.

`Player.getFlying()` - проверяет, летит ли игрок.

`Player.setFlying(fly?)` - устанавливает состояние полета игрока.

`Player.exhaustion()` - возвращает состояние истощения игрока.

`Player.getExhaustion()` - возвращает истощение игрока.

`Player.setExhaustion(exh)` - задает игроку exh единиц истощения.

`Player.hunger()` - возвращает состояние сытости игрока.

`Player.getHunger()` - возвращает сытость игрока.

`Player.setHunger(hnr)` - задает hnr единиц сытости игрока.

`Player.saturation()` - возвращает состояние насыщения игрока.

`Player.getSaturation()` - возвращает насыщение игрока.

`Player.setSaturation(sat)` - задает sat единиц насыщения игроку.

`Player.health()` - возвращает состояние здоровья игрока.

`Player.getHealth()` - возвращает здоровье игрока.

`Player.setHealth(hp)` - задает hp единиц здоровья игроку.

`Player.score()` - возвращает состояние счета игрока.

`Player.getScore()` - возвращает счет игрока.
