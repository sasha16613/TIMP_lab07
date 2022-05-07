# TIMP_lab07

Hunter подключает библиотеки к данному репозиторию, которые находятся в другом репозитории

* Для нашей лабораторной работы нам понядобятся 3 репозитория.
    1. репозиторий №1) (https://github.com/sasha16613/print) репозиторий, откуда мы будем брать библиотеку
    2. репозиторий hunter) (https://github.com/cpp-pm/hunter) делаем fork, чтобы он появился в нашем списке репозиториев на github
    3. репозиторий №2) (https://github.com/sasha16613/TIMP_lab07) репозиторий, который использует библиотеку из репозитория №1

## репозиторий №1
1. В папках include и source пишется наша функция, здесь ничего сложного.
2. Перейдем в CMakeLists.txt и пробежимся по самому главному.
    1. В строках с 6 по 12 собирается наш проект.
    2. 14-17 - происходит переименование для сокращения кода.
    3. 19-25 - Вспомогательные функции для создания файлов конфигурации, которые могут быть включены другими 
    проектами для поиска и использования пакета.
    5. 27-39 - Создание бинарных файлов для отправки их в другой репозиторий.
* информацию по папке cmake и строкам 19 - 25 в CMakeLists.txt можно найти по ссылке: 
https://cmake.org/cmake/help/latest/module/CMakePackageConfigHelpers.html

1. Делаем релиз проекта и копируем ссылку на архив с расширением tar.gz
2. переходим в консоль и пишем wget <ссылка> - скачивает архив
3. пишем в консоли openssl sha1 <название архива> - создается хэш, который нужен для hunter

## репозиторий hunter
1. После того, как мы сделали fork хантера, нужно создать свою ветку (в моем случае это ветка branch).
2. Переходим в папку cmake -> projects там создаем папку с названием вашего проекта (print - папка моего проекта). 
Внутри папки создаем файл hunter.cmake.
В hunter.cmake мы пишем:
![image alt](https://github.com/sasha16613/images/blob/main/Безымянный%20(2).png)
    1. 7 - Имя проекта
    2. 9 - Сюда пишем тэг, который использовался при релизе репозитория №1.
    3. 11 - Передаем ссылку на архив (которую вводили в wget)
    4. 13 - Пишем хэш архива (получили благодаря openssl)
    5. 17 - Используется для выбора схемы сборки для текущего проекта. 
        * https://hunter.readthedocs.io/en/latest/reference/user-modules/hunter_pick_scheme.html
    6. 18 - Команда даст разрешение на пакет, чтобы его можно было сохранить в кеше. 
        * https://hunter.readthedocs.io/en/latest/reference/user-modules/hunter_cacheable.html
    7. 19 - Команда прочитает все переменные, связанные с пакетом, и запустит настоящие инструкции по загрузке/сборке
        * https://hunter.readthedocs.io/en/latest/reference/user-modules/hunter_download.html
3. Возвращаемся в папку cmake, переходим в папку configs -> default.cmake. Туда пишем hunter_default_version(<PACKAGE_NAME> VERSION <...>) 
"данные берем из hunter.cmake, который мы писали на предыдущем шаге.
    * hunter_default_version нужно писать в алфавитном порядке, по PACKAGE_NAME

* повторяем действия с релизом, которые мы делали в репозиторий №1.

## репозиторий №2
1. Благодаря cmake/HunterGate.cmake, мы можем использовать hunter. Этот файл копируется из репозитория hunter (gate/cmake/HunterGate.cmake).
    * https://hunter.readthedocs.io/en/latest/quick-start/boost-components.html
    * https://github.com/cpp-pm/gate/blob/958e65c65fb659293b23c7a3c64abf3ce81e6ff1/cmake/HunterGate.cmake
2. В CMakeLists.txt:
    1. 5 - этот модуль автоматически загрузит архив с указанного вами URL-адреса.
    2. 7-10 - передаем ссылку на архив и его хэш.
    3. 14 - устанавливает
    4. 15 - теперь можно использовать
       * https://hunter.readthedocs.io/en/latest/creating-new/create/cmake.html
3. .github/workflows: просто билдит проект

* ссылка на документацию для hunter: https://hunter.readthedocs.io/en/latest/index.html
