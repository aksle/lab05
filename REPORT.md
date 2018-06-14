## Laboratory work IV

Данная лабораторная работа посвещена изучению систем автоматизации сборки проекта на примере **CMake**

```ShellSession
$ open https://cmake.org/
```

## Tasks

- [ ] 1. Создать публичный репозиторий с названием **lab04** на сервисе **GitHub**
- [ ] 2. Ознакомиться со ссылками учебного материала
- [ ] 3. Выполнить инструкцию учебного материала
- [ ] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```ShellSession
$ export GITHUB_USERNAME=<имя_пользователя>
```
lll
```ShellSession
$ cd ${GITHUB_USERNAME}/workspace # переходим в директорию
$ pushd . # сохраняем имя текущего каталога для команды popd
$ source scripts/activate # выполняем команды из файла


```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab03.git projects/lab04 # получаем копию репозитория
Cloning into 'projects/lab04'...
remote: Counting objects: 15, done.
remote: Compressing objects: 100% (10/10), done.
remote: Total 15 (delta 1), reused 9 (delta 0), pack-reused 0
Unpacking objects: 100% (15/15), done.
$ cd projects/lab04 # переходим в папку
$ git remote remove origin # удаляем доступ к предыдущему удаленному репозиторию
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab04.git # добавляем новый удаленный репозитори
```

```ShellSession
$ g++ -I./include -std=c++11 -c sources/print.cpp # компилируем файл исходного кода 
$ ls print.o # выводим на экран содержание файла с объектным кодом
print.o 
$ nm print.o | grep print # выводим на экран строки объектного файла, содержащие print
$ ar rvs print.a print.o # архивируем объектный файл
ar: creating print.a
a - print.o
$ file print.a # определяем тип файла и понимаем, что архивирование прошло успешно
print.a: current ar archive
$ g++ -I./include -std=c++11 -c examples/example1.cpp # компилируем файл исходного кода
$ ls example1.o # выводим на экран содержание файла с объектным кодом
example1.o
$ g++ example1.o print.a -o example1 # компилируем объектный файл с функцией main и заархивированный ранее файл  
$ ./example1 && echo # запускаем скомпилированный файл и выводим строку текста
hello
```

```ShellSession
$ g++ -I./include -std=c++11 -c examples/example2.cpp # компилируем второй файл исходного кода с функцией main
$ nm example2.o # выводим на экран бинарное содержимое объектного файла
$ g++ example2.o print.a -o example2 # компилиркуем файл, содержащий main, и архив
$ ./example2 # запускаем программу
$ cat log.txt && echo # выводим на экран содержимое файла в единый поток
hello
```

```ShellSession
$ rm -rf example1.o example2.o print.o # удаляем файлы объектного кода
$ rm -rf print.a # удаляем архив
$ rm -rf example1 example2 # удаляем скомпилированные файлы
$ rm -rf log.txt # удаляем файл txt
```

```ShellSession
$ cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.0)
project(print)
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
EOF
```
```ShellSession
$ cat >> CMakeLists.txt <<EOF
add_library(print STATIC \${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF
include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/include)
EOF
```

```ShellSession
$ cmake -H. -B_build # подготовливаеи процесс построения проекта, проверяем работоспособность и записываем результат в файл
$ cmake --build _build # создаем сгенерированное ранее двоичное дерево проекта

```

```ShellSession
$ cat >> CMakeLists.txt <<EOF

add_executable(example1 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example1.cpp)
add_executable(example2 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example2.cpp)
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF

target_link_libraries(example1 print)
target_link_libraries(example2 print)
EOF
```

```ShellSession
$ cmake --build _build # запускаем сборку в текущем каталоге
$ cmake --build _build --target print # запускаем сборку с целью print вместо цели по умолчанию
$ cmake --build _build --target example1
$ cmake --build _build --target example2
```

```ShellSession
$ ls -la _build/libprint.a # выводим содержимое файла с расширением .a (статический файл библиотеки)
$ _build/example1 && echo # выполняем сборку и выводим на экран
hello
$ _build/example2 # выполняем сборку
$ cat log.txt && echo # выводим на экран содержимое файла в единый поток
hello
$ rm -rf log.txt # удаляем файл
```

```ShellSession
$ git clone https://github.com/tp-labs/lab04 tmp # клонируем репозиторий
Cloning into 'tmp'...
remote: Counting objects: 37, done.
remote: Total 37 (delta 0), reused 0 (delta 0), pack-reused 37
Unpacking objects: 100% (37/37), done.
$ mv -f tmp/CMakeLists.txt . # перемещаем файл в текущую директорию
$ rm -rf tmp # удаляем клонированную ранее директорию
```

```ShellSession
$ cat CMakeLists.txt
$ cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
$ cmake --build _build --target install
$ tree _install
```

```ShellSession
$ git add CMakeLists.txt # начинаем отслеживание файла
$ git commit -m"added CMakeLists.txt" # создаем коммит
$ git push origin master # отправляем ветку на сервер
```

## Report

```ShellSession
$ popd # возвращаемся в каталог, ранее запомненный командой popd
$ export LAB_NUMBER=04 # присваиваем переменной окружения занчение
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER} # клонируем репозиторий
$ mkdir reports/lab${LAB_NUMBER} # создаем директорию
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md # копируем содержимое файлов
$ cd reports/lab${LAB_NUMBER} # переходим в каталог
$ edit REPORT.md # открываем файл на чтение
$ gistup -m "lab${LAB_NUMBER}" # загружаем отчет 
```

## Links

- [Autotools](http://www.gnu.org/software/automake/manual/html_node/Autotools-Introduction.html)
- [CMake](https://cgold.readthedocs.io/en/latest/index.html)

```
Copyright (c) 2017 Братья Вершинины
```
