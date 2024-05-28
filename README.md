## Homework

Представьте, что вы стажер в компании "Formatter Inc.".
### Задание 1
Вам поручили перейти на систему автоматизированной сборки **CMake**.
Исходные файлы находятся в директории [formatter_lib](formatter_lib).
В этой директории находятся файлы для статической библиотеки *formatter*.
Создайте `CMakeList.txt` в директории [formatter_lib](formatter_lib),
с помощью которого можно будет собирать статическую библиотеку *formatter*.

```sh
$ cd formatter_lib
```

```sh
$ cat >> CMakeLists.txt << EOF
cmake_minimum_required(VERSION 3.4)
project(formatter_lib)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
add_library(formatter_lib STATIC ${CMAKE_CURRENT_SOURCE_DIR}/formatter.cpp)
EOF
```

```sh
$ cmake -B build
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
-- Build files have been written to: /home/vboxuser/Desktop/aakx000/workspace/labs/lab03/formatter_lib/build
```

```sh
$ cmake --build build
[ 50%] Building CXX object CMakeFiles/formatter_lib.dir/formatter.cpp.o
[100%] Linking CXX static library libformatter_lib.a
[100%] Built target formatter_lib
```

### Задание 2
У компании "Formatter Inc." есть перспективная библиотека,
которая является расширением предыдущей библиотеки. Т.к. вы уже овладели
навыком созданием `CMakeList.txt` для статической библиотеки *formatter*, ваш 
руководитель поручает заняться созданием `CMakeList.txt` для библиотеки 
*formatter_ex*, которая в свою очередь использует библиотеку *formatter*.

```sh
$ cat >> CMakeLists.txt << EOF
> cmake_minimum_required(VERSION 3.4)
> project(formatter_ex_lib)
> set(CMAKE_CXX_STANDARD 11)
> set(CMAKE_CXX_STANDARD_REQUIRED ON)
> add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib formatter_lib_dir)
> add_library(formatter_ex_lib STATIC ${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex.cpp)
> target_include_directories(formatter_ex_lib PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib)
> target_link_libraries(formatter_ex_lib formatter_lib)
> EOF
```

```sh
$ cmake -B build
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
-- Build files have been written to: /home/vboxuser/Desktop/aakx000/workspace/labs/lab03/formatter_ex_lib/build
```

```sh
$ cmake --build build
[ 25%] Building CXX object formatter_lib_dir/CMakeFiles/formatter_lib.dir/formatter.cpp.o
[ 50%] Linking CXX static library libformatter_lib.a
[ 50%] Built target formatter_lib
[ 75%] Building CXX object CMakeFiles/formatter_ex_lib.dir/formatter_ex.cpp.o
[100%] Linking CXX static library libformatter_ex_lib.a
[100%] Built target formatter_ex_lib
```

### Задание 3
Конечно же ваша компания предоставляет примеры использования своих библиотек.
Чтобы продемонстрировать как работать с библиотекой *formatter_ex*,
вам необходимо создать два `CMakeList.txt` для двух простых приложений:
* *hello_world*, которое использует библиотеку *formatter_ex*;
* *solver*, приложение которое испольует статические библиотеки *formatter_ex* и *solver_lib*.

```sh
$ cat >> CMakeLists.txt << EOF
cmake_minimum_required(VERSION 3.4)
project(hello_world)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib formatter_ex_lib_dir)
add_executable(hello_world ${CMAKE_CURRENT_SOURCE_DIR}/hello_world.cpp)
target_include_directories(hello_world PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib)
target_link_libraries(hello_world formatter_ex_lib)
EOF
```

```sh
$ cmake -B build
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
-- Build files have been written to: /home/vboxuser/Desktop/aakx000/workspace/labs/lab03/hello_world_application/build
```

```sh
$ cmake --build build
[ 16%] Building CXX object formatter_ex_lib_dir/formatter_lib_dir/CMakeFiles/formatter_lib.dir/formatter.cpp.o
[ 33%] Linking CXX static library libformatter_lib.a
[ 33%] Built target formatter_lib
[ 50%] Building CXX object formatter_ex_lib_dir/CMakeFiles/formatter_ex_lib.dir/formatter_ex.cpp.o
[ 66%] Linking CXX static library libformatter_ex_lib.a
[ 66%] Built target formatter_ex_lib
[ 83%] Building CXX object CMakeFiles/hello_world.dir/hello_world.cpp.o
[100%] Linking CXX executable hello_world
[100%] Built target hello_world
```

```sh
$ build/hello_world
-------------------------
hello, world!
-------------------------
```

в папке solver_lib:
```sh
$ cat >> CMakeLists.txt << EOF
cmake_minimum_required(VERSION 3.4)
project(solver_lib)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
add_library(solver_lib STATIC ${CMAKE_CURRENT_SOURCE_DIR}/solver.cpp)
EOF
```

```sh
$ cmake -B build
$ cmake -B build
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
-- Build files have been written to: /home/vboxuser/Desktop/aakx000/workspace/labs/lab03/solver_lib/build
```

```sh
$ cmake --build build
[ 50%] Building CXX object CMakeFiles/solver_lib.dir/solver.cpp.o
/home/vboxuser/Desktop/aakx000/workspace/labs/lab03/solver_lib/solver.cpp: In function ‘void solve(float, float, float, float&, float&)’:
/home/vboxuser/Desktop/aakx000/workspace/labs/lab03/solver_lib/solver.cpp:14:21: error: ‘sqrtf’ is not a member of ‘std’; did you mean ‘strtof’?
   14 |     x1 = (-b - std::sqrtf(d)) / (2 * a);
      |                     ^~~~~
      |                     strtof
/home/vboxuser/Desktop/aakx000/workspace/labs/lab03/solver_lib/solver.cpp:15:21: error: ‘sqrtf’ is not a member of ‘std’; did you mean ‘strtof’?
   15 |     x2 = (-b + std::sqrtf(d)) / (2 * a);
      |                     ^~~~~
      |                     strtof
gmake[2]: *** [CMakeFiles/solver_lib.dir/build.make:76: CMakeFiles/solver_lib.dir/solver.cpp.o] Error 1
gmake[1]: *** [CMakeFiles/Makefile2:83: CMakeFiles/solver_lib.dir/all] Error 2
gmake: *** [Makefile:91: all] Error 2
```

```sh
$ vim solver.cpp
```

```sh
#include "solver.h"

#include <stdexcept>
#include <cmath>

void solve(float a, float b, float c, float& x1, float& x2)
{
    float d = (b * b) - (4 * a * c);

    if (d < 0)
    {
        throw std::logic_error{"error: discriminant < 0"};
    }

    x1 = (-b - sqrtf(d)) / (2 * a);
    x2 = (-b + sqrtf(d)) / (2 * a);
}
```

```sh
$ cmake -B build
-- Configuring done
-- Generating done
-- Build files have been written to: /home/vboxuser/Desktop/aakx000/workspace/labs/lab03/solver_lib/build
$ cmake --build build
Consolidate compiler generated dependencies of target solver_lib
[ 50%] Building CXX object CMakeFiles/solver_lib.dir/solver.cpp.o
[100%] Linking CXX static library libsolver_lib.a
[100%] Built target solver_lib
```

в папке solver_application:
```sh
cat >> CMakeLists.txt << EOF
cmake_minimum_required(VERSION 3.4)
project(solver)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib formatter_ex_lib_dir)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../solver_lib solver_lib_dir)
add_executable(solver ${CMAKE_CURRENT_SOURCE_DIR}/equation.cpp)
target_include_directories(solver PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib ${CMAKE_CURRENT_SOURCE_DIR}/../solver_lib)
target_link_libraries(solver formatter_ex_lib solver_lib)
EOF
```

```sh
$ cmake -B build
-- Configuring done
-- Generating done
-- Build files have been written to: /home/vboxuser/Desktop/aakx000/workspace/labs/lab03/solver_application/build
```

```sh
$ cmake --build build
[ 12%] Building CXX object solver_lib_dir/CMakeFiles/solver_lib.dir/solver.cpp.o
[ 25%] Linking CXX static library libsolver_lib.a
[ 25%] Built target solver_lib
[ 37%] Building CXX object formatter_ex_lib_dir/formatter_lib_dir/CMakeFiles/formatter_lib.dir/formatter.cpp.o
[ 50%] Linking CXX static library libformatter_lib.a
[ 50%] Built target formatter_lib
[ 62%] Building CXX object formatter_ex_lib_dir/CMakeFiles/formatter_ex_lib.dir/formatter_ex.cpp.o
[ 75%] Linking CXX static library libformatter_ex_lib.a
[ 75%] Built target formatter_ex_lib
[ 87%] Building CXX object CMakeFiles/solver.dir/equation.cpp.o
[100%] Linking CXX executable solver
[100%] Built target solver
```

```sh
$ build/solver
1
2
-10
-------------------------
x1 = -4.316625
-------------------------
-------------------------
x2 = 2.316625
-------------------------
```

```
Copyright (c) 2015-2021 The ISC Authors
```
