# Что это?

Это очередная "стюардесса", а именно - скрипт, который обрабатывает выгрузку из реестра в формате .xml и перекладывает ее в базу данных MySQL для последующих экзерсисов.

Выкладываю потому, что в 2018 году нельзя пытаться "для быстроты" парсить XML при помощи регулярок. Это даже хуже, чем стрелять себе в ногу.

Код далек от прекрасного, но лучше, чем регулярки.

# Зависимости?

* PHP 5+ (наверное, точно не знаю :) c драйвером для MySQL, конечно
* [SimpleXMLReader](https://github.com/dkrnl/SimpleXMLReader) - включен в комплект
* [DbSimple](https://github.com/ivan1986/DbSimple) - это форк оригинальной [DbSimple от Дмитрия Котерова](https://github.com/DmitryKoterov/DbSimple) - тоже включен

# А как ее ~~раскопать~~ настроить?
Все, что нужно указать - это реквизиты доступа к базе и саму базу данных. Указываются в файле сonfig.

Скрипт автоматически создает таблицу с именем registryYYYYMMDD, пихает туда данные.

# Как запускать?

```./registry.import dump.xml```

# А насколько быстро работает?

Дамп в 39 мегабайт разбирает в среднем за полторы минуты.

maniaque@decadence:~/registry.git⟫ time ./registry.import dump.xml
registry20180419

real    1m25.894s
user    0m36.112s
sys     0m12.944s

# А что она в stdout выводит?

Имя созданной таблицы с данными о блокированных объектах. Это удобно потом использовать для дальнейших скриптов.

Таблица каждый раз очищается, но при помощи транзакций. Поэтому если скрипт прервать в середине, то ничего страшного не случится, предыдущую версию не заденет.

# Структуру таблицы можно в студию?

Канеш.

```CREATE TABLE IF NOT EXISTS `registry20180419` (
  `registry_id` int(11) NOT NULL AUTO_INCREMENT,
  `content_id` int(11) NOT NULL,
  `block_type` char(11) NOT NULL,
  `type` char(6) NOT NULL,
  `value` text NOT NULL,
   PRIMARY KEY (`registry_id`) USING BTREE,
   INDEX (`content_id`),
   INDEX (`type`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE utf8_general_ci;```
