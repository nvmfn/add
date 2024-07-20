Создание блока крипов (Creature Block) в Dota 2 с использованием Dota 2 Workshop Tools включает несколько шагов: настройку Lua скриптов, создание нужных файлов и их конфигурацию. Creature Block позволяет вам управлять спавном крипов, их волнами и поведением. Вот пошаговая инструкция, как это сделать:

Шаг 1: Настройка файла npc_units_custom.txt
Перейдите в папку вашего проекта и откройте файл npc_units_custom.txt или создайте его, если он не существует.
Добавьте описание крипов, которых вы хотите использовать в блоке. Вот пример:
plaintext
```
"npc_units_custom"
{
    "npc_dota_creep_custom_melee"
    {
        "BaseClass"                     "npc_dota_creep_lane"
        "Model"                         "models/creeps/lane_creeps/creep_radiant_melee/radiant_melee.vmdl"
        "SoundSet"                      "Creep_Melee"
        "Level"                         "1"
        "IsAncient"                     "0"
        "AttackCapabilities"            "DOTA_UNIT_CAP_MELEE_ATTACK"
        "AttackDamageMin"               "21"
        "AttackDamageMax"               "26"
        "AttackRate"                    "1.0"
        "ArmorPhysical"                 "2"
        "MagicalResistance"             "0"
        "MovementSpeed"                 "325"
        "HealthBarOffset"               "140"
        "BountyGoldMin"                 "35"
        "BountyGoldMax"                 "45"
        "StatusHealth"                  "550"
        "StatusHealthRegen"             "2.0"
    }
}
```
Шаг 2: Создание блока крипов
Создайте файл creature_block.txt в папке вашего проекта, например, в scripts/npc/.
plaintext
```
"CreatureBlock"
{
    "Wave1"
    {
        "UnitName"    "npc_dota_creep_custom_melee"
        "Count"       "5"
        "SpawnDelay"  "30.0" // Время задержки перед спавном (в секундах)
        "Waypoint"    "path_corner_1"
    }
    "Wave2"
    {
        "UnitName"    "npc_dota_creep_custom_melee"
        "Count"       "5"
        "SpawnDelay"  "60.0" // Время задержки перед спавном (в секундах)
        "Waypoint"    "path_corner_2"
    }
}
```
Шаг 3: Настройка пути крипов
Перейдите в папку вашего проекта и создайте файл path_corner_custom.txt.
Добавьте описание путей:
plaintext
```
"path_corner_custom"
{
    "path_corner_1"
    {
        "Position"  "0 0 0"  // Укажите координаты первой точки пути
        "Next"      "path_corner_2"
    }
    "path_corner_2"
    {
        "Position"  "100 100 0"  // Укажите координаты второй точки пути
        "Next"      "path_corner_3"
    }
    "path_corner_3"
    {
        "Position"  "200 200 0"  // Укажите координаты третьей точки пути
        "Next"      ""
    }
}
```
Шаг 4: Настройка Lua скриптов
Перейдите в папку вашего проекта и создайте или откройте файл Lua скриптов (обычно это addon_game_mode.lua).
Добавьте функцию для спавна крипов по волнам.
lua
```
function SpawnWave(wave)
    local spawnLocation = Entities:FindByName(nil, wave.Waypoint):GetAbsOrigin()
    for i = 1, wave.Count do
        local unit = CreateUnitByName(wave.UnitName, spawnLocation, true, nil, nil, DOTA_TEAM_GOODGUYS)
        unit:SetInitialGoalEntity(Entities:FindByName(nil, wave.Waypoint))
    end
end

function InitializeWaves()
    local waveData = LoadKeyValues("scripts/npc/creature_block.txt")
    for waveName, wave in pairs(waveData) do
        Timers:CreateTimer(tonumber(wave.SpawnDelay), function()
            SpawnWave(wave)
        end)
    end
end

function Activate()
    GameRules.AddonTemplate = CAddonTemplate()
    GameRules.AddonTemplate:InitGameMode()
end

function CAddonTemplate:InitGameMode()
    print("Dota 2 Custom Game Loaded.")
    InitializeWaves()
end
```
Шаг 5: Размещение путей на карте
Откройте вашу карту в Hammer Editor (редактор карт).
Создайте энтити путей (path_corner) и установите их позиции на карте, соединяя их друг с другом в соответствии с вашим файлом path_corner_custom.txt.
Шаг 6: Тестирование
Сохраните все изменения и запустите ваш проект через Dota 2 Workshop Tools.
Откройте вашу карту и проверьте, что крипы спавнятся правильно по заданным волнам и движутся по путям.
