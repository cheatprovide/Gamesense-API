# Документация по Gamesense Lua API

---

## 📋 Оглавление

1. [Обзор](#обзор)
2. [Основные модули](#основные-модули)
3. [Начало работы](#начало-работы)
4. [Справочник API](#справочник-api)
5. [Примеры кода](#примеры-кода)
6. [Лучшие практики](#лучшие-практики)

---

## Обзор

API Gamesense Lua предоставляет обширный набор инструментов, специально разработанных для создания скриптов. API обеспечивают глубокий доступ к состоянию игры, отрисовку оверлеев для улучшения видимости и управление пользовательским интерфейсом для настраиваемых чит-функций и т.д. . Так же API упрощает разработку скриптов для улучшения игрового процесса, включая анти-аим, резольвер и визуальные функции, а также позволяет отлаживать и настраивать чит-компоненты для максимальной эффективности.

### Ключевые возможности

- **Система обратных вызовов** — Архитектура, основанная на событиях
- **Рендеринг и визуализация** — Рисование текста, фигур и текстур на экране
- **Манипуляция сущностями** — Получение и изменение данных игроков и объектов
- **Управление интерфейсом** — Создание своих элементов меню и интерфейса
- **Постоянное хранение** — Система базы данных для сохранения настроек
- **Информация о сети** — Мониторинг задержки и состояния очереди команд
- **Обработка столкновений** — Трассировка и определение видимости хитбоксов

---

## Основные модули

| Модуль            | Назначение                                     |
|-------------------|------------------------------------------------|
| **client**        | Клиентские функции (события, логирование)      |
| **entity**        | Манипуляция игроками и сущностями              |
| **globals**       | Глобальные переменные и игровой тайминг        |
| **ui**            | Элементы меню и интерфейса                     |
| **renderer**      | Рендеринг экрана и графика                     |
| **config**        | Управление конфигурациями и пресетами          |
| **cvar**          | Работа с переменными консоли                   |
| **database**      | Система постоянного хранения данных            |
| **materialsystem**| Манипуляция материалами и шейдерами            |
| **plist**         | Управление списком игроков                     |
| **panorama**      | Работа с UI панелями и JavaScript              |

---

## Начало работы

### Простейший скрипт

Любой скрипт Gamesense строится по принципу:

```lua
-- Создаём элемент интерфейса
local my_feature = ui.new_checkbox("MISC", "Main", "Включить функцию")

-- Подписка на событие
client.set_event_callback("paint", function()
    if ui.get(my_feature) then
        -- Логика функции тут
        client.log("Функция активна!")
    end
end)

-- Лог при загрузке
client.log("Скрипт успешно загружен!")
```

### Работа с событиями

Весь код исполняется через обратные вызовы событий:

```lua
client.set_event_callback(event_name, callback_function)
```

**Распространённые события:**
- `paint` — срабатывает каждый кадр для рендеринга
- Игровые события — пользовательские события от игрового движка

Удалить обработчик:

```lua
client.unset_event_callback(event_name, callback_function)
```

---

## Справочник API

### Клиентские функции

#### События

```lua
client.set_event_callback(event_name, callback)
client.unset_event_callback(event_name, callback)
```

---

#### Логирование

```lua
client.log(msg, ...)
client.color_log(r, g, b, msg, ...)
client.error_log(msg)
```

---

#### Команды консоли

```lua
client.exec(cmd, ...)
```

---

#### Ввод пользователя

```lua
client.key_state(virtual_key)
```

---

#### Временные функции

```lua
client.latency()
client.timestamp()
client.system_time()
client.unix_time()
```

---

#### Камера

```lua
client.camera_angles()
client.camera_position()
client.eye_position()
client.screen_size()
```

---

#### Проверка видимости и трассировка

```lua
client.visible(x, y, z)
client.trace_line(skip_entindex, from_x, from_y, from_z, to_x, to_y, to_z)
client.trace_bullet(from_player, from_x, from_y, from_z, to_x, to_y, to_z, skip_players)
```

---

#### Расчёт урона

```lua
client.scale_damage(entindex, hitgroup, damage)
```

---

#### Отладочный рендеринг

```lua
client.draw_debug_text(x, y, z, line_offset, duration, r, g, b, a, ...)
client.draw_hitboxes(entindex, duration, hitboxes, r, g, b, a, tick)
```

---

#### Генерация случайных чисел

```lua
client.random_int(minimum, maximum)
client.random_float(minimum, maximum)
```

---

#### Прочие функции

```lua
client.userid_to_entindex(userid)
client.set_clan_tag(...)
client.delay_call(delay, callback, ...)
client.reload_active_scripts()
```

---

#### Низкоуровневый доступ

```lua
client.create_interface(module_name, interface_name)
client.find_signature(module_name, pattern)
client.get_model_name(model_index)
client.register_esp_flag(flag, r, g, b, callback)
```

---

### Функции сущности

#### Получение сущностей

```lua
entity.get_local_player()
entity.get_all(classname)
entity.get_players(enemies_only)
entity.get_game_rules()
entity.get_player_resource()
```

---

#### Свойства сущности

```lua
entity.get_classname(ent)
entity.get_prop(ent, propname, array_index)
entity.set_prop(ent, propname, value, array_index)
entity.get_origin(player)
entity.get_esp_data(player)
```

---

#### Информация об игроке

```lua
entity.get_player_name(ent)
entity.get_player_weapon(ent)
entity.get_steam64(player)
entity.is_enemy(ent)
entity.is_alive(ent)
entity.is_dormant(ent)
```

---

#### Hitbox

```lua
entity.hitbox_position(player, hitbox)
entity.get_bounding_box(player)
```

---

### Функции рендеринга

**Важно:** большинство функций вызываются только из события `paint`.

#### Отрисовка текста

```lua
renderer.text(x, y, r, g, b, a, flags, max_width, ...)
renderer.measure_text(flags, ...)
renderer.indicator(r, g, b, a, ...)
```

---

#### Отрисовка фигур

```lua
renderer.rectangle(x, y, w, h, r, g, b, a)
renderer.line(xa, ya, xb, yb, r, g, b, a)
renderer.gradient(x, y, w, h, r1, g1, b1, a1, r2, g2, b2, a2, ltr)
renderer.triangle(x0, y0, x1, y1, x2, y2, r, g, b, a)
renderer.circle(x, y, r, g, b, a, radius, start_degrees, percentage)
renderer.circle_outline(x, y, r, g, b, a, radius, start_degrees, percentage, thickness)
```

---

#### Отрисовка текстур

```lua
renderer.texture(id, x, y, w, h, r, g, b, a, mode)
renderer.load_svg(contents, width, height)
renderer.load_png(contents, width, height)
renderer.load_jpg(contents, width, height)
renderer.load_rgba(contents, width, height)
```

---

#### Перевод координат

```lua
renderer.world_to_screen(x, y, z)
```

---

### Интерфейс

#### Элементы интерфейса

```lua
ui.new_checkbox(tab, container, name)
ui.new_slider(tab, container, name, min, max, init_value, show_tooltip, unit, scale, tooltips)
ui.new_combobox(tab, container, name, ...)
ui.new_multiselect(tab, container, name, ...)
ui.new_hotkey(tab, container, name, inline, default_hotkey)
ui.new_button(tab, container, name, callback)
ui.new_color_picker(tab, container, name, r, g, b, a)
ui.new_textbox(tab, container, name)
ui.new_listbox(tab, container, name, items)
ui.new_label(tab, container, name)
ui.new_string(name, value)
```

**Основные вкладки:** `RAGE`, `AA`, `LEGIT`, `VISUALS`, `MISC`, `SKINS`, `PLAYERS`, `LUA`

---

#### Управление интерфейсом

```lua
ui.get(item)
ui.set(item, value, ...)
ui.set_callback(item, callback)
ui.set_visible(item, visible)
ui.reference(tab, container, name)
```

---

#### Статус интерфейса

```lua
ui.is_menu_open()
ui.mouse_position()
ui.menu_position()
ui.menu_size()
ui.name(item)
```

---

### Конфигурация

```lua
config.load(name, tab_name, container_name)
config.export()
```

---

### Переменные консоли

```lua
cvar.set_string(value)
cvar.get_string()
cvar.set_float(value)
cvar.set_raw_float(value)
cvar.get_float()
cvar.set_int(value)
cvar.set_raw_int(value)
cvar.get_int()
cvar.invoke_callback(...)
```

---

### Глобальные переменные

```lua
globals.realtime()
globals.curtime()
globals.frametime()
globals.absoluteframetime()
globals.maxplayers()
globals.tickcount()
globals.tickinterval()
globals.framecount()
globals.mapname()
globals.lastoutgoingcommand()
globals.oldcommandack()
globals.commandack()
globals.chokedcommands()
```

---

### Система материалов

```lua
materialsystem.find_material(path, force_load)
materialsystem.find_materials(partial_path, force_load)
materialsystem.find_texture(path)
materialsystem.get_model_materials(entindex)
-- Свойства материала
materialsystem.get_name()
materialsystem.reload()
materialsystem.color_modulate(r, g, b)
materialsystem.alpha_modulate(alpha)
materialsystem.get_shader_param(param_name)
materialsystem.set_shader_param(param_name, value, force)
materialsystem.get_material_var_flag(index)
materialsystem.set_material_var_flag(index, enabled)
-- Специальные материалы
materialsystem.arms_material()
materialsystem.chams_material()
```

---

### База данных

```lua
database.write(key, value)
database.read(key)
```

---

### Panorama UI

```lua
panorama.open(panel)
panorama.loadstring(js_code, panel)
```

---

### Список игроков

```lua
plist.set(entindex, field, value)
plist.get(entindex, field)
```

---

## Примеры кода

### Пример 1: Переключение Wallhack

```lua
local wallhack_enabled = ui.new_checkbox("VISUALS", "World", "Wallhack")
client.set_event_callback("paint", function()
    if not ui.get(wallhack_enabled) then return end
    for _, player in ipairs(entity.get_players(true)) do
        if entity.is_alive(player) then
            local x, y, z = entity.get_origin(player)
            renderer.world_to_screen(x, y, z)
        end
    end
end)
```

### Пример 2: Обработка горячей клавиши

```lua
local my_hotkey = ui.new_hotkey("MISC", "Main", "Toggle Feature", true)
client.set_event_callback("paint", function()
    if ui.get(my_hotkey) then
        client.log("Hotkey was pressed!")
    end
end)
```

### Пример 3: Отображение отладочной информации

```lua
client.set_event_callback("paint", function()
    local mouse_x, mouse_y = ui.mouse_position()
    local screen_w, screen_h = ui.screen_size()
    renderer.text(10, 10, 255, 255, 255, 255, nil, 0, "Mouse: " .. mouse_x .. ", " .. mouse_y)
    renderer.text(10, 30, 255, 255, 255, 255, nil, 0, "Resolution: " .. screen_w .. "x" .. screen_h)
    renderer.text(10, 50, 255, 255, 255, 255, nil, 0, "FPS: " .. math.floor(1 / globals.frametime()))
end)
```

### Пример 4: Имена игроков на ESP

```lua
client.set_event_callback("paint", function()
    local me = entity.get_local_player()
    if not me then return end
    for _, player in ipairs(entity.get_players(true)) do
        if entity.is_alive(player) then
            local name = entity.get_player_name(player)
            local enemy_x, enemy_y, enemy_z = entity.get_origin(player)
            local screen_x, screen_y = renderer.world_to_screen(enemy_x, enemy_y, enemy_z)
            if screen_x then
                renderer.text(screen_x, screen_y, 255, 0, 0, 255, "c", 0, name)
            end
        end
    end
end)
```

### Пример 5: Сохранение данных

```lua
database.write("my_settings", {feature_enabled = true, sensitivity = 42, color = {255, 128, 0}})
local saved_settings = database.read("my_settings")
if saved_settings then
    client.log("Загружена чувствительность: " .. saved_settings.sensitivity)
end
```

---

## Лучшие практики

### 1. Проверяйте значения на nil

```lua
local pos = entity.get_origin(player)
if pos then
    local x, y, z = pos
end
```

### 2. Правильное использование событий

```lua
local my_callback = function()
    -- Код события
end
client.set_event_callback("paint", my_callback)
client.unset_event_callback("paint", my_callback)
```

### 3. Оптимизация кода в paint

```lua
client.set_event_callback("paint", function()
    if globals.framecount() % 10 == 0 then
        -- Исполняется раз в 10 кадров
    end
    client.delay_call(1, function()
        -- Исполняется через 1 секунду
    end)
end)
```

### 4. Постоянное хранение данных

```lua
ui.set_callback(my_setting, function()
    database.write("user_settings", {my_setting = ui.get(my_setting)})
end)
```

### 5. Используйте константы цветов

```lua
local colors = {
    red = {255, 0, 0, 255},
    green = {0, 255, 0, 255},
    blue = {0, 0, 255, 255},
    white = {255, 255, 255, 255}
}
renderer.text(x, y, colors.red[1], colors.red[2], colors.red[3], colors.red[4], nil, 0, "Ошибка")
```

### 6. Валидация пользовательского ввода

```lua
local slider_value = ui.get(my_slider)
if slider_value >= 0 and slider_value <= 100 then
    -- Можно использовать
else
    client.error_log("Недопустимое значение!")
end
```

### 7. Трассировка для проверки видимости

```lua
local fraction, hit = client.trace_line(0, x1, y1, z1, x2, y2, z2)
if fraction == 1.0 then
    -- Прямая видимость
else
    -- Что-то мешает обзору
end
```

### 8. Корректная работа с сущностями

```lua
local local_player = entity.get_local_player()
if not local_player then return end
for _, player in ipairs(entity.get_players()) do
    if entity.is_alive(player) and not entity.is_dormant(player) then
        -- Допустимая обработка
    end
end
```

---

*Вопросы по API? Дискорд: discord.gg/vusCGG8BUB*
