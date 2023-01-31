# 🎋 Source File Analyzer
Консольное приложение, с помощью которого можно быстро проанализировать зависимости в исходных файлах. 
## О проекте

ПО написано с применением архитектурного паттерна Model-View-Presenter. И легко адаптируется под любой язык программирования.
<p align="center">
  <img src="https://github.com/RNOVOSELOV/sources_analyzer/blob/main/images/UML.svg"/>
</p>

Результат работы программы - дерево зависимостей файлов, а также частота включений. Анализируемые файлы должны иметь расширения ".h", ".hpp", либо ".cpp" для С++ исходников; ".dart" для Dart.

## Работа ПО
Анализатор - консольное приложение. Для запуска необходимо выполнить команду:
> analyser <PATH> [опции]

В качестве PATH указывается путь до анализируемого файла. 
Если PATH это директория, то дерево зависимостей строится для всех файлов внутри каталога и его поддиректорий.

Возможные опции:

-	-I путь для поиска библиотечных заголовков


Пример вызова программы:

> analyser c:\mysources\ -I c:\mysources\includes -I c:\mylibrary
> analyser -I c:\mysources\includes c:\mysources\main.cpp -I c:\mylibrary c:\mysources\
> analyser -I c:\mysources\includes -I c:\mylibrary c:\mysources\main.cpp

<p align="center">
  <img src="https://github.com/RNOVOSELOV/sources_analyzer/blob/main/images/result_1.png" height="300"/>
</p>

Если в дереве уже присутствует добавляемый PATH, то при выдаче результата он помечается как REP. Дальнейший проход внутрь файла не осуществляется. 
Благодаря этому уменьшается дерево за счет исключения дублирующих зависимостей, а так же устраняется проблема «циклических» зависимостей. 

Метка DEL ставится у файлов, которые не удалось обнаружить на файловой системе. Для локальных заголовочных файлов в каталоге с файлом, для библиотечных в одном из путей переданных с флагом -I.

## Развитие проекта

### Версия 1.1

- Абстракция модели, возможность быстрой адаптации под другие языки программирования.
- Расширение списка опций, возможность анализировать отдельные файлы

### Версия 1.0

Minimal Viable Product - построение дерева зависимостей для всех файлов в каталоге, отображение списка частот включений файлов

## Индикация ошибок

#### Возможные ошибки:

- Некорректный запуск программы

    > Error! Number of arguments error. Source path isn't setted.
	
- Не указан анализируемый каталог или файл
	
    > Error! Source's path not exist on filesystem.

#### Предупреждения:

- Через флаг -I указана несуществующая на файловой системе директория 

    > WARNING! Directory \<directory path\> doesn’t exist!
  
- В исходном файле обнаружен библиотечный заголовочный файл, отсутствующий в директориях, перечисленных с помощью опции -I. В дереве такой заголовочный файл помечается как DEL.
  
    > WARNING! In \<filename\>: header \<library header\> not found in -I directories!
