Для изменения изображения экрана загрузки в Dota 2 с использованием Dota 2 Workshop Tools, необходимо выполнить следующие шаги:

Шаг 1: Подготовка изображения
Создайте или выберите изображение для экрана загрузки. Оно должно быть в формате .png и иметь размер 1920x1080 пикселей для обеспечения наилучшего качества.
Сохраните это изображение в папке вашего проекта, например, в папке materials/overviews.
Шаг 2: Настройка VMT и VTF файлов
Для отображения изображения в игре, необходимо создать файлы .vmt и .vtf.

Создание VTF файла:

Скачайте и установите VTFEdit.
Откройте ваше изображение в VTFEdit и сохраните его как .vtf файл в папке materials/overviews вашего проекта.
Создание VMT файла:

Создайте текстовый файл в той же папке, что и ваш .vtf файл, и назовите его так же, как и ваш .vtf файл, но с расширением .vmt.
Откройте этот файл и добавьте следующий код:
plaintext
Копировать код
```
"UnlitGeneric"
{
    "$basetexture" "materials/overviews/your_image_name"
    "$translucent" 1
    "$ignorez" 1
}
```
Шаг 3: Обновление addon_game_mode.vcfg
Вам нужно указать в конфигурации вашего мода, что вы хотите использовать собственный экран загрузки.

Откройте файл addon_game_mode.vcfg в папке вашего проекта.
Добавьте следующую строку:
plaintext
```
"CustomUIConfig"
{
    "hud/loadingscreen" "file://{resources}/flash/your_custom_loading_screen.swf"
}
```
Шаг 4: Обновление panorama файлов
Для использования экрана загрузки через Panorama, вам нужно настроить соответствующие файлы.

Перейдите в папку вашего проекта content/panorama/layout/custom_game/.
Создайте или откройте файл custom_loading_screen.xml.
Добавьте следующий код:
xml
```
<root>
    <styles>
        <include src="file://{resources}/styles/custom_game/custom_loading_screen.css" />
    </styles>
    <Panel style="width: 100%; height: 100%;">
        <Image id="LoadingScreenImage" src="file://{images}/your_image_name.vtf" scaling="stretch-to-fit-preserve-aspect" />
    </Panel>
</root>
```
Перейдите в папку вашего проекта content/panorama/styles/custom_game/.
Создайте или откройте файл custom_loading_screen.css.
Добавьте следующий код:
css
```
#LoadingScreenImage
{
    width: 100%;
    height: 100%;
    opacity: 1.0;
}
```
Шаг 5: Тестирование изменений
Сохраните все изменения и запустите ваш проект через Dota 2 Workshop Tools.
Убедитесь, что ваш экран загрузки отображается правильно при запуске игры.
