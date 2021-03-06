# ElvUI_Profile

## Что делает плагин

Что бы, при смене аккаунта/персонажа вам не нужно было копировать настройки 
вашего профиля из папки SavedVariables вы можете скопировать настройки сюда. 
В результате, заходя на любого персонажа, на любом аккаунте, вы сможете нажать 
одну кнопку и все настройки скопируются и применятся.

Это пример плагина для профиля ElvUI. Аналогичным образом можно добавить любые 
аддоны. Да, вам придется немного почитать код и напрячь мозг, зато потраченное время
с лихвой окупиться. 

Как это выглядит в игре:
 * Заходим в настройки ElvUI 
 * Видим наш плагин My profiles (Функция GetOptions() добавляет 3 кнопки: myUI, noname, return_name)
 * Нажимаем на myUI(это и есть наш профиль) + reloadUI
 * Готово

Так же я добавила 2 функции: 
 * noname() - Заменяет ники персонажей на название класса(меняет только ники в профиле, не перезаписывая его полностью)
 * return_name() - Возвращает ники

## How to 

 * Не пытайтесь применить мой профиль, в него входят плагины, которых у вас нет,
это вызавет ошибки. Вам нужно скопировать и вставить ваш профиль между строк:
<pre><code>
--Ваши настройки 
... Тут  нужно удалить мой профиль и вставить свой
--Ваши настройки конец
</code></pre>

 * Каждая настройка должна быть отдельной строкой, дабы при обновлении аддонов не 
поломать профиль. перезаписав таблицу настроек полностью. Т.е. нельзя просто
скопировать профиль из папки wtf и вставить его в виде:
<pre><code>
E.db = {
    ["unitframe"] = {
        ["units"] = {
            ["party"] = {
                ["roleIcon"] = {
                    ["xOffset"] = 1,
                    ["size"] = 14,
                    ["position"] = "TOPLEFT",
                    ["yOffset"] = 4,
                }
            }
         }       
    }
}
</code></pre> - это означет полную перезапись таблицы E.db. Если аддон добавить какую-то функцию, 
то такая запись будет её стирать. это может привести к ошибкам. 
Правильно записывать так:
<pre><code>
E.db["unitframe"]["units"]["party"]["roleIcon"]["xOffset"] = 1
E.db["unitframe"]["units"]["party"]["roleIcon"]["size"] = 14
E.db["unitframe"]["units"]["party"]["roleIcon"]["position"] = "TOPLEFT"
E.db["unitframe"]["units"]["party"]["roleIcon"]["yOffset"] = 4
</code></pre>

 * Для этого в ElvUI есть удобный экспорт профиля. В формате экспорта выберите "плагин" 
и экспорт будет построчный.

 * Если вы используете customTexts в настройках юнитфреймов, то перед строками с 
["customTexts"] вам нужно будет добавить строки с названием текста, иначе будет ругань.
<pre><code>
Пример:
E.db["unitframe"]["units"]["party"]["customTexts"]["dead"] = {} -- добавленная строка
E.db["unitframe"]["units"]["party"]["customTexts"]["dead"]["attachTextTo"] = "HEALTH"
</code></pre>

 * Функция замены ников на названия классов. меняем [name:short](или [name:medium]) на [class]
<pre><code>
E.db["unitframe"]["units"]["raid"]["name"]["text_format"] = "[namecolor][name:short]"
</code></pre>
на
<pre><code>
E.db["unitframe"]["units"]["raid"]["name"]["text_format"] = "[namecolor][class]"
</code></pre>
Если вы используете ["customTexts"] для имени, то код будет выглядеть примерно так:
<pre><code>
E.db["unitframe"]["units"]["target"]["name"]["text_format"] = ""
E.db["unitframe"]["units"]["target"]["customTexts"]["lvlName"]["text_format"] = "[difficultycolor][level] [classification] [namecolor][class]"
</code></pre>	

### P.S. 

Оригинальный плагин написан [этим](http://shadowmage.ru/about/) замечательным человеком. [Тут](https://www.youtube.com/watch?v=U6ZV_sPVYno&index=1&list=PLfgzSuaNSSQUh8EfwARn9_mWTXi1X0Gtx) есть его подробное видео, 
объясняющее принцип работы всех строк