# Создание технической документации. Краткая инструкция


## Установка Docker на OC Windows 10

Для написания технической документации с дальнейшим экспортом в формат *PDF* предлагается
воспользоваться процессором [asciidoctor](https://asciidoctor.org), но перед использования данного
процессора необходима установка большого количества стороннего ПО. С целью упрощения задачи предлагается
использование пакета [asciidoctor/docker-asciidoctor](https://hub.docker.com/r/asciidoctor/docker-asciidoctor)
Для этого требуется установка на ПК платформу [Docker](https://asciidoctor.org).

Перейдя по [ссылке](https://docs.docker.com/docker-for-windows/install) вы окажетесь на странице
скачивания установочного файла. Для его скачивания кликните на _download.docker.com_ как показано на
рисункax [Ссылка на скачивание](https://github.com/Alyaksej/cetthemes/blob/master/example/img/1.png)
и [Начало процесса скачивания](https://github.com/Alyaksej/cetthemes/blob/master/example/img/2.png).
По завершении процесса скачивания необходимо начать процесс установки дважды
кликнув на скачанный файл [Начало процесса установки](https://github.com/Alyaksej/cetthemes/blob/master/example/img/3.png).
Когда установка завершится, Docker запустится автоматически.

![Ссылка на скачивание](https://github.com/Alyaksej/cetthemes/blob/master/example/img/1.png)
![Начало процесса скачивания](https://github.com/Alyaksej/cetthemes/blob/master/example/img/2.png)
![Начало процесса установки](https://github.com/Alyaksej/cetthemes/blob/master/example/img/3.png)

##	Скачивание ползовательского шаблона.

Для скачивания на ПК пользовательской темы перейдите по [ссылке](https://github.com/Alyaksej/CET_docThemes)
После перехода на GitHub скачайте репозиторий одним архивом на свой ПК. Распакуйте данный архив. Можно приступать
к заполнению документаю

![Скачивание репозитория](https://github.com/Alyaksej/cetthemes/blob/master/example/img/4.png)

##	Порядок заполнения документа

Для корректного заполнения документа необходимо создать файл *main.adoc* следующего содержания:

```
:my-title: Название документа                           //Заполняется разработчиком
:my-subtitle: Подзаглавие                               //Заполняется разработчиком
:city-date: Минск 2019                                  //Заполняется разработчиком
= {my-title}: {my-subtitle}                                  //Остается без изменений
:notitle:                                                    //Остается без изменений
:date: 29.04.2019                                       //Заполняется разработчиком (дата создания документа)
:version: 0.0.1                                         //Заполняется разработчиком
:author_name: Семашко Алексей                           //Заполняется разработчиком
:lang: ru                                               // ru/en - зависит от языка докмента
:front-cover-image: title.pdf                                //Остается без изменений
:parametres_path: resources/themes/parametres/{lang}         //Остается без изменений
include::{parametres_path}/parametres.adoc[lines1..34]       //Остается без изменений
include::{parametres_path}/author.adoc[]                     //Остается без изменений

```

Далее следует заполнение документа заполнение документа. Чтобы не перегружать файл *main.adoc*,
предлагается использовать вспомогательные файлы при помощи директивы *#include*, для этого предлагается
создать в томже каталоге, в котором находится файл *main.adoc* каталоги *includes* (для хранения включаемых
файлов расширения _.adoc_) каталог *img* для хранения изображений.
Эти папки в месте с файлом *main.adoc* поместить в каталог, который мы получили после распаковки архива.
Если данные каталоги уже существуют, можно просто очистить их содержимое и поместить в них
свои файлы.

Подключение файлов к файлу *main.adoc* имеет вид:

```
include::path/to/file/including.adoc[leveloffset=+1]

где
include::         //директива препроцессора
path/to/file/     //путь к файлу
including.adoc    //название файла
[leveloffset=+1]  //смещение уровня (для присвоение каждой главе заголовка первого уровня)

если файл *main.adoc* находится в тойже папке, что и каталог *includes* подключение будет иметь вид

include::includes/including.adoc[leveloffset=+1]
```

Кажлдый новый файл следуе начинать с указания пути к включаемым файлам кода и изображений:

`:imagesdir: img  //Указание пути к изображениям`


Далее следуют таблица "Версия документа", в ней всё, что заключена в {} скобки  оставляем без изменений.

```
== {version-doc}

[cols="3,4,4,10a", options="header"]
|===
| {version-tab}     | {date-tab}   | {author-tab}   | {amendment-tab}
.<| {version}     .<| {date}     .<| {author}     .<| {date}
|===

```

##	Экспорт в формат PDF

Для перевода документа в PDF формат необходимо открыть терминал перейти в каталог
в котором находятся файлы _main.adoc_ и _title.adoc_ (cetthemes/) как указано на рисунке 
![](https://github.com/Alyaksej/cetthemes/blob/master/example/img/5.png).

В зависимости от операционной системы, которой вы пользуетесь нужно ввести в командную строку
одну из следующих команд:


!!!!!ВАЖНО!!!!! Избегайте в названиях файлов и путей к ним кирилических букв!!!!!

Linux:
`docker run --rm -v $(pwd):/documents/ asciidoctor/docker-asciidoctor asciidoctor-pdf title.adoc main.adoc`

Windows:
`docker run --rm -v %cd%:/documents/ asciidoctor/docker-asciidoctor asciidoctor-pdf title.adoc main.adoc`


![](https://github.com/Alyaksej/cetthemes/blob/master/example/img/6.png)

После появления файлов PDF следует переименовать _main.pdf_, удалить файл _title.pdf_ и
очистить содержимое папок *includes* и *img*.
