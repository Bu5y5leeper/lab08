## Homework 

Представьте, что вы стажер в компании "Formatter Inc.".

### Задание 1
Вам поручили перейти на систему автоматизированной сборки **CMake**.
Исходные файлы находятся в директории [formatter_lib](formatter_lib).
В этой директории находятся файлы для статической библиотеки *formatter*.
Создайте `CMakeList.txt` в директории [formatter_lib](formatter_lib),
с помощью которого можно будет собирать статическую библиотеку *formatter*.

```bash
cmake --version
```
```
cmake version 3.22.1

CMake suite maintained and supported by Kitware (kitware.com/cmake).
```
---

### Содержимое formatter_lib/CMakeLists.txt:
```
cmake_minimum_required(VERSION 3.22) # Проверка версии CMake.
									 # Если версия установленой программы
									 # старее указаной, произайдёт аварийный выход.

project(formatter_lib)				# Название проекта

set(SOURCE_LIB formatter.cpp formatter.h)		# Установка переменной со списком исходников

add_library(formatter_lib STATIC ${SOURCE_LIB}) # Создание статической библиотеки
```
---
```bash
cmake -H. -B_build
cmake --build ./_build
```

### Задание 2
У компании "Formatter Inc." есть перспективная библиотека,
которая является расширением предыдущей библиотеки. Т.к. вы уже овладели
навыком созданием `CMakeList.txt` для статической библиотеки *formatter*, ваш 
руководитель поручает заняться созданием `CMakeList.txt` для библиотеки 
*formatter_ex*, которая в свою очередь использует библиотеку *formatter*.

```bash
cd ../formatter_ex_lib/
```
---
### Содержимое formatter_ex_lib/CMakeLists.txt:
```
cmake_minimum_required(VERSION 3.22)

project(formatter_ex_lib)

set(SOURCE_LIB formatter_ex.cpp)

add_library(formatter_ex_lib STATIC ${SOURCE_LIB})

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib formatter_lib)

target_link_libraries(formatter_ex_lib PUBLIC formatter_lib)
target_include_directories(formatter_ex_lib PUBLIC
				"${CMAKE_CURRENT_SOURCE_DIR}"
				"${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib")

```

### Задание 3
Конечно же ваша компания предоставляет примеры использования своих библиотек.
Чтобы продемонстрировать как работать с библиотекой *formatter_ex*,
вам необходимо создать два `CMakeList.txt` для двух простых приложений:
* *hello_world*, которое использует библиотеку *formatter_ex*;
* *solver*, приложение которое испольует статические библиотеки *formatter_ex* и *solver_lib*.
### Hello_world_application
```bash
cd ../formatter_ex_lib/
```
---
### Содержимое hello_world_application/CMakeLists.txt:
```
cmake_minimum_required(VERSION 3.22) # Проверка версии CMake.
									 # Если версия установленой программы
									 # старее указаной, произайдёт аварийный выход.

project(hello_world)				 # Название проекта

set(SOURCE_EXE hello_world.cpp)			 # Установка переменной со списком исходников

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib/)

add_executable(main ${SOURCE_EXE})	 # Создает исполняемый файл с именем main

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib/ formatter_ex_lib)

target_link_libraries(main formatter_ex_lib)		 # Линковка программы с библиотекой
```

### Solver_application
```bash
cd ../solver_application/
```
### Содержимое solver_application/CMakeLists.txt:
```
cmake_minimum_required(VERSION 3.22) # Проверка версии CMake.
									# Если версия установленой программы
									# старее указаной, произайдёт аварийный выход.

project(solver)				# Название проекта

set(SOURCE_EXE equation.cpp)			# Установка переменной со списком исходников

include_directories("${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib/"
					"${CMAKE_CURRENT_SOURCE_DIR}/../solver_lib/")

add_executable(main ${SOURCE_EXE})	# Создает исполняемый файл с именем main

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib/ formatter_ex_lib)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../solver_lib/ solver_lib)

target_link_libraries(main formatter_ex_lib solver_lib)		# Линковка программы с библиотекой
```
```bash
cd ../solver_lib/
```
---
### Содержимое solver_lib/CMakeLists.txt:
```
cmake_minimum_required(VERSION 3.22) # Проверка версии CMake.
									# Если версия установленой программы
									# старее указаной, произайдёт аварийный выход.

project(solver_lib)				# Название проекта

set(SOURCE_LIB solver.cpp solver.h)		# Установка переменной со списком исходников

add_library(solver_lib STATIC ${SOURCE_LIB})# Создание статической библиотеки
```

### Компиляция проекта
1. formatter_lib
```bash
cmake -H. -B_build
cmake --build ./_build
```
```
-- The C compiler identification is GNU 11.4.0
-- The CXX compiler identification is GNU 11.4.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/bu5y/Bu5y5leeper/workspace/projects/lab03_dz/formatter_lib/_build
```
---
2. formatter_ex_lib
```bash
cmake -H. -B_build
cmake --build ./_build
```
```
-- The C compiler identification is GNU 11.4.0
-- The CXX compiler identification is GNU 11.4.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/bu5y/Bu5y5leeper/workspace/projects/lab03_dz/formatter_ex_lib/_build
```
---
3. hello_world
```bash
cmake -H. -B_build
cmake --build ./_build
```
```
-- The C compiler identification is GNU 11.4.0
-- The CXX compiler identification is GNU 11.4.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/bu5y/Bu5y5leeper/workspace/projects/lab03_dz/hello_world_application/_build
```
4. solver_lib
```bash
cmake -H. -B_build
cmake --build ./_build
```
```
-- The C compiler identification is GNU 11.4.0
-- The CXX compiler identification is GNU 11.4.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/bu5y/Bu5y5leeper/workspace/projects/lab03_dz/solver_lib/_build
```
5. solver_application
```bash
cmake -H. -B_build
cmake --build ./_build
```
```
-- The C compiler identification is GNU 11.4.0
-- The CXX compiler identification is GNU 11.4.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/bu5y/Bu5y5leeper/workspace/projects/lab03_dz/solver_application/_build
```
```
Copyright (c) 2015-2021 The ISC Authors
```
