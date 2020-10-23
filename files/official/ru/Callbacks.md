# Кэлбеки

!> 本文档是为旧的 Inner Core 1.0 或更旧的 Core Engine 编写的。参阅时请仔细分辨，以免造成不必要的麻烦。

ReadSaves - чтение всех сохранений  
WriteSaves - запись всех сохранений

GenerateNetherChunk (chunkX, chunkZ)  
GenerateEndChunk (chunkX, chunkZ)  
GenerateChunk (chunkX, chunkZ)  
GenerateChunkUndeground (chunkX, chunkZ)

PreRestart - перед перезапуском bl

tick - тик  
LevelSelected - начало загрузки мира  
LevelLoaded - конец загрузки мира  
LevelLeft - выход из мира

DestroyBlock (coords, block, player) - уничтожение блока (coords.relative, coords.side)  
DestroyBlockStart (coords, block, player) - начало уничтожения блока  
DestroyBlockContinue (coords, block, progress, player) - продолжение уничтожения блока 3 в тик  
Explosion (coords, params) - взрыв params.entity, params.power, params.onFire  
RedstoneSignal (coords, params, block) - params.power, params.booleanPar  
NativeCommand (str) - команда  
ClientChat (str) - сообщение  
ServerChat (str) -"-

ItemUse (coords, item, block) - coords.relative, coords.side  
PlayerAttack (player, victim) - атака игрока  
EntityAdded (entity) - добаление моба  
EntityRemoved (entity) - удаление моба  
EntityDeath (entity) - смерть  
EntityHurt (attacker, victim, damage) - получение урона мобом

ProjectileHit (projectile, item, target) - (item.id, item.count, item.data), (target.x, target.y, target.z, if block hit - target.coords, if entity hit - target.entity)  
ProjectileHitBlock (projectile, coords, block)  
ProjectileHitEntity (projectile, entity)

FoodEaten (heal, satRatio)  
NativeGuiChanged (screenName)  
ExpAdd (amount, player)  
ExpLevelAdd (levelAmount, player)

ModDirLoaded - сразу после загрузки модов  
PreBlocksDefined - перед добавлением блоков  
BlocksDefined - добаление блоков  
PreLoaded - перед окончательной загрузкой  
APILoaded - для загрузки API модов  
ModsLoaded - еще один  
PostLoaded - окончательная загрузка всего
