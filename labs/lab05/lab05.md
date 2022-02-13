# Lab 5

### Part 1

###### Steps 1 & 2

<img src="https://i.imgur.com/lPkHqbS.png" width=700>

<details>
  <summary> Code </summary> 
  <br>
  
  tutorial.cxx
  
  ```
// A simple program that computes the square root of a number
#include <cmath>
#include <iostream>
#include <string>

#include "TutorialConfig.h"

#ifdef USE_MYMATH
#  include "MathFunctions.h"
#endif

int main(int argc, char* argv[])
{
  if (argc < 2) {
    // report version
    std::cout << argv[0] << " Version " << Tutorial_VERSION_MAJOR << "."
              << Tutorial_VERSION_MINOR << std::endl;
    std::cout << "Usage: " << argv[0] << " number" << std::endl;
    return 1;
  }

  // convert input to double
  const double inputValue = std::stod(argv[1]);

  // calculate square root
  #ifdef USE_MYMATH
    const double outputValue = mysqrt(inputValue);
  #else
    const double outputValue = sqrt(inputValue);
  #endif
  std::cout << "The square root of " << inputValue << " is " << outputValue
            << std::endl;
  return 0;
}
```

CMakeLists.txt

```
cmake_minimum_required(VERSION 3.10)

# set the project name and version
project(Tutorial VERSION 1.0)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

option(USE_MYMATH "Use tutorial provided math implementation" ON)

# configure a header file to pass some of the CMake settings
# to the source code
configure_file(TutorialConfig.h.in TutorialConfig.h)

if(USE_MYMATH)
  add_subdirectory(MathFunctions)
  list(APPEND EXTRA_LIBS MathFunctions)
  list(APPEND EXTRA_INCLUDES "${PROJECT_SOURCE_DIR}/MathFunctions")
endif()

# add the executable
add_executable(Tutorial tutorial.cxx)

target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS})

# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
target_include_directories(Tutorial PUBLIC
                           "${PROJECT_BINARY_DIR}"
                           ${EXTRA_INCLUDES}
                           )
 ```
 
 TutorialConfig.h.in
 
 ```
 // the configured options and settings for Tutorial
#define Tutorial_VERSION_MAJOR @Tutorial_VERSION_MAJOR@
#define Tutorial_VERSION_MINOR @Tutorial_VERSION_MINOR@
#cmakedefine USE_MYMATH
```
</details>

###### Step 3

<img src="https://i.imgur.com/KCrISG2.png" width=700>
<details>
<summary> Code </summary>
<br>

tutorial.cxx

```
// A simple program that computes the square root of a number
#include <cmath>
#include <iostream>
#include <string>

#include "TutorialConfig.h"

// should we include the MathFunctions header?
#ifdef USE_MYMATH
#  include "MathFunctions.h"
#endif

int main(int argc, char* argv[])
{
  if (argc < 2) {
    // report version
    std::cout << argv[0] << " Version " << Tutorial_VERSION_MAJOR << "."
              << Tutorial_VERSION_MINOR << std::endl;
    std::cout << "Usage: " << argv[0] << " number" << std::endl;
    return 1;
  }

  // convert input to double
  const double inputValue = std::stod(argv[1]);

  // which square root function should we use?
#ifdef USE_MYMATH
  const double outputValue = mysqrt(inputValue);
#else
  const double outputValue = sqrt(inputValue);
#endif

  std::cout << "The square root of " << inputValue << " is " << outputValue
            << std::endl;
  return 0;
}
```

CMakeLists.txt

```
cmake_minimum_required(VERSION 3.10)

# set the project name and version
project(Tutorial VERSION 1.0)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# should we use our own math functions
option(USE_MYMATH "Use tutorial provided math implementation" ON)

# configure a header file to pass some of the CMake settings
# to the source code
configure_file(TutorialConfig.h.in TutorialConfig.h)

# add the MathFunctions library
if(USE_MYMATH)
  add_subdirectory(MathFunctions)
  list(APPEND EXTRA_LIBS MathFunctions)
endif()

# add the executable
add_executable(Tutorial tutorial.cxx)

target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS})

# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
target_include_directories(Tutorial PUBLIC
                           "${PROJECT_BINARY_DIR}"
                           )
```

TutorialConfig.h.in

```
// the configured options and settings for Tutorial
#define Tutorial_VERSION_MAJOR @Tutorial_VERSION_MAJOR@
#define Tutorial_VERSION_MINOR @Tutorial_VERSION_MINOR@
#cmakedefine USE_MYMATH
```

MathFunctions/CMakeLists.txt

```
add_library(MathFunctions mysqrt.cxx)

target_include_directories(MathFunctions
          INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}
          )
```
</details>

###### Step 4

<details>

<summary> ctest -VV output </summary>

```
(base) nick-lennoxs-MacBook-Pro:Step4_build nick-lennox$ ctest -VV
UpdateCTestConfiguration  from :/Users/nick-lennox/Desktop/cmake/Help/guide/tutorial/Step4_build/DartConfiguration.tcl
UpdateCTestConfiguration  from :/Users/nick-lennox/Desktop/cmake/Help/guide/tutorial/Step4_build/DartConfiguration.tcl
Test project /Users/nick-lennox/Desktop/cmake/Help/guide/tutorial/Step4_build
Constructing a list of tests
Done constructing a list of tests
Updating test list for fixtures
Added 0 tests to meet fixture requirements
Checking test dependency graph...
Checking test dependency graph end
test 1
    Start 1: Runs

1: Test command: /Users/nick-lennox/Desktop/cmake/Help/guide/tutorial/Step4_build/Tutorial "25"
1: Test timeout computed to be: 10000000
1: Computing sqrt of 25 to be 13
1: Computing sqrt of 25 to be 7.46154
1: Computing sqrt of 25 to be 5.40603
1: Computing sqrt of 25 to be 5.01525
1: Computing sqrt of 25 to be 5.00002
1: Computing sqrt of 25 to be 5
1: Computing sqrt of 25 to be 5
1: Computing sqrt of 25 to be 5
1: Computing sqrt of 25 to be 5
1: Computing sqrt of 25 to be 5
1: The square root of 25 is 5
1/9 Test #1: Runs .............................   Passed    0.05 sec
test 2
    Start 2: Usage

2: Test command: /Users/nick-lennox/Desktop/cmake/Help/guide/tutorial/Step4_build/Tutorial
2: Test timeout computed to be: 10000000
2: /Users/nick-lennox/Desktop/cmake/Help/guide/tutorial/Step4_build/Tutorial Version 1.0
2: Usage: /Users/nick-lennox/Desktop/cmake/Help/guide/tutorial/Step4_build/Tutorial number
2/9 Test #2: Usage ............................   Passed    0.00 sec
test 3
    Start 3: Comp4

3: Test command: /Users/nick-lennox/Desktop/cmake/Help/guide/tutorial/Step4_build/Tutorial "4"
3: Test timeout computed to be: 10000000
3: Computing sqrt of 4 to be 2.5
3: Computing sqrt of 4 to be 2.05
3: Computing sqrt of 4 to be 2.00061
3: Computing sqrt of 4 to be 2
3: Computing sqrt of 4 to be 2
3: Computing sqrt of 4 to be 2
3: Computing sqrt of 4 to be 2
3: Computing sqrt of 4 to be 2
3: Computing sqrt of 4 to be 2
3: Computing sqrt of 4 to be 2
3: The square root of 4 is 2
3/9 Test #3: Comp4 ............................   Passed    0.00 sec
test 4
    Start 4: Comp9

4: Test command: /Users/nick-lennox/Desktop/cmake/Help/guide/tutorial/Step4_build/Tutorial "9"
4: Test timeout computed to be: 10000000
4: Computing sqrt of 9 to be 5
4: Computing sqrt of 9 to be 3.4
4: Computing sqrt of 9 to be 3.02353
4: Computing sqrt of 9 to be 3.00009
4: Computing sqrt of 9 to be 3
4: Computing sqrt of 9 to be 3
4: Computing sqrt of 9 to be 3
4: Computing sqrt of 9 to be 3
4: Computing sqrt of 9 to be 3
4: Computing sqrt of 9 to be 3
4: The square root of 9 is 3
4/9 Test #4: Comp9 ............................   Passed    0.00 sec
test 5
    Start 5: Comp5

5: Test command: /Users/nick-lennox/Desktop/cmake/Help/guide/tutorial/Step4_build/Tutorial "5"
5: Test timeout computed to be: 10000000
5: Computing sqrt of 5 to be 3
5: Computing sqrt of 5 to be 2.33333
5: Computing sqrt of 5 to be 2.2381
5: Computing sqrt of 5 to be 2.23607
5: Computing sqrt of 5 to be 2.23607
5: Computing sqrt of 5 to be 2.23607
5: Computing sqrt of 5 to be 2.23607
5: Computing sqrt of 5 to be 2.23607
5: Computing sqrt of 5 to be 2.23607
5: Computing sqrt of 5 to be 2.23607
5: The square root of 5 is 2.23607
5/9 Test #5: Comp5 ............................   Passed    0.00 sec
test 6
    Start 6: Comp7

6: Test command: /Users/nick-lennox/Desktop/cmake/Help/guide/tutorial/Step4_build/Tutorial "7"
6: Test timeout computed to be: 10000000
6: Computing sqrt of 7 to be 4
6: Computing sqrt of 7 to be 2.875
6: Computing sqrt of 7 to be 2.65489
6: Computing sqrt of 7 to be 2.64577
6: Computing sqrt of 7 to be 2.64575
6: Computing sqrt of 7 to be 2.64575
6: Computing sqrt of 7 to be 2.64575
6: Computing sqrt of 7 to be 2.64575
6: Computing sqrt of 7 to be 2.64575
6: Computing sqrt of 7 to be 2.64575
6: The square root of 7 is 2.64575
6/9 Test #6: Comp7 ............................   Passed    0.00 sec
test 7
    Start 7: Comp25

7: Test command: /Users/nick-lennox/Desktop/cmake/Help/guide/tutorial/Step4_build/Tutorial "25"
7: Test timeout computed to be: 10000000
7: Computing sqrt of 25 to be 13
7: Computing sqrt of 25 to be 7.46154
7: Computing sqrt of 25 to be 5.40603
7: Computing sqrt of 25 to be 5.01525
7: Computing sqrt of 25 to be 5.00002
7: Computing sqrt of 25 to be 5
7: Computing sqrt of 25 to be 5
7: Computing sqrt of 25 to be 5
7: Computing sqrt of 25 to be 5
7: Computing sqrt of 25 to be 5
7: The square root of 25 is 5
7/9 Test #7: Comp25 ...........................   Passed    0.00 sec
test 8
    Start 8: Comp-25

8: Test command: /Users/nick-lennox/Desktop/cmake/Help/guide/tutorial/Step4_build/Tutorial "-25"
8: Test timeout computed to be: 10000000
8: The square root of -25 is 0
8/9 Test #8: Comp-25 ..........................   Passed    0.00 sec
test 9
    Start 9: Comp0.0001

9: Test command: /Users/nick-lennox/Desktop/cmake/Help/guide/tutorial/Step4_build/Tutorial "0.0001"
9: Test timeout computed to be: 10000000
9: Computing sqrt of 0.0001 to be 0.50005
9: Computing sqrt of 0.0001 to be 0.250125
9: Computing sqrt of 0.0001 to be 0.125262
9: Computing sqrt of 0.0001 to be 0.0630304
9: Computing sqrt of 0.0001 to be 0.0323084
9: Computing sqrt of 0.0001 to be 0.0177018
9: Computing sqrt of 0.0001 to be 0.0116755
9: Computing sqrt of 0.0001 to be 0.0101202
9: Computing sqrt of 0.0001 to be 0.0100007
9: Computing sqrt of 0.0001 to be 0.01
9: The square root of 0.0001 is 0.01
9/9 Test #9: Comp0.0001 .......................   Passed    0.00 sec

100% tests passed, 0 tests failed out of 9

Total Test time (real) =   0.08 sec
```

</details>

<details>
<summary>CMakeLists.txt</summary>

```
enable_testing()

# does the application run
add_test(NAME Runs COMMAND Tutorial 25)

# does the usage message work?
add_test(NAME Usage COMMAND Tutorial)
set_tests_properties(Usage
  PROPERTIES PASS_REGULAR_EXPRESSION "Usage:.*number"
  )

# define a function to simplify adding tests
function(do_test target arg result)
  add_test(NAME Comp${arg} COMMAND ${target} ${arg})
  set_tests_properties(Comp${arg}
    PROPERTIES PASS_REGULAR_EXPRESSION ${result}
    )
endfunction(do_test)

# do a bunch of result based tests
do_test(Tutorial 4 "4 is 2")
do_test(Tutorial 9 "9 is 3")
do_test(Tutorial 5 "5 is 2.236")
do_test(Tutorial 7 "7 is 2.645")
do_test(Tutorial 25 "25 is 5")
do_test(Tutorial -25 "-25 is [-nan|nan|0]")
do_test(Tutorial 0.0001 "0.0001 is 0.01")

cmake_minimum_required(VERSION 3.10)

# set the project name and version
project(Tutorial VERSION 1.0)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# should we use our own math functions
option(USE_MYMATH "Use tutorial provided math implementation" ON)

# configure a header file to pass some of the CMake settings
# to the source code
configure_file(TutorialConfig.h.in TutorialConfig.h)

# add the MathFunctions library
if(USE_MYMATH)
  add_subdirectory(MathFunctions)
  list(APPEND EXTRA_LIBS MathFunctions)
endif()

# add the executable
add_executable(Tutorial tutorial.cxx)

target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS})

# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
target_include_directories(Tutorial PUBLIC
                           "${PROJECT_BINARY_DIR}"
                           )

install(TARGETS Tutorial DESTINATION bin)
install(FILES "${PROJECT_BINARY_DIR}/TutorialConfig.h"
  DESTINATION include
  )
```
</details>

<details>
<summary> MathFunctions/CMakeLists.txt </summary>

```
add_library(MathFunctions mysqrt.cxx)

# state that anybody linking to us needs to include the current source dir
# to find MathFunctions.h, while we don't.
target_include_directories(MathFunctions
          INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}
          )

install(TARGETS MathFunctions DESTINATION lib)
install(FILES MathFunctions.h DESTINATION include)
```
</details>

###### Step 5

<img src="https://i.imgur.com/I6WOzYP.png">

<details>
<summary> Code </summary>

CMakeLists.txt

```
cmake_minimum_required(VERSION 3.10)

# set the project name and version
project(Tutorial VERSION 1.0)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# should we use our own math functions
option(USE_MYMATH "Use tutorial provided math implementation" ON)

# configure a header file to pass some of the CMake settings
# to the source code
configure_file(TutorialConfig.h.in TutorialConfig.h)

# add the MathFunctions library
if(USE_MYMATH)
  add_subdirectory(MathFunctions)
  list(APPEND EXTRA_LIBS MathFunctions)
endif()

# add the executable
add_executable(Tutorial tutorial.cxx)
target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS})

# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
target_include_directories(Tutorial PUBLIC
                           "${PROJECT_BINARY_DIR}"
                           )

# add the install targets
install(TARGETS Tutorial DESTINATION bin)
install(FILES "${PROJECT_BINARY_DIR}/TutorialConfig.h"
  DESTINATION include
  )

# enable testing
enable_testing()

# does the application run
add_test(NAME Runs COMMAND Tutorial 25)

# does the usage message work?
add_test(NAME Usage COMMAND Tutorial)
set_tests_properties(Usage
  PROPERTIES PASS_REGULAR_EXPRESSION "Usage:.*number"
  )

# define a function to simplify adding tests
function(do_test target arg result)
  add_test(NAME Comp${arg} COMMAND ${target} ${arg})
  set_tests_properties(Comp${arg}
    PROPERTIES PASS_REGULAR_EXPRESSION ${result}
    )
endfunction(do_test)

# do a bunch of result based tests
do_test(Tutorial 4 "4 is 2")
do_test(Tutorial 9 "9 is 3")
do_test(Tutorial 5 "5 is 2.236")
do_test(Tutorial 7 "7 is 2.645")
do_test(Tutorial 25 "25 is 5")
do_test(Tutorial -25 "-25 is [-nan|nan|0]")
do_test(Tutorial 0.0001 "0.0001 is 0.01")
```

MathFunctions/CMakeLists.txt

```
include(CheckSymbolExists)
check_symbol_exists(log "math.h" HAVE_LOG)
check_symbol_exists(exp "math.h" HAVE_EXP)
if(NOT (HAVE_LOG AND HAVE_EXP))
  unset(HAVE_LOG CACHE)
  unset(HAVE_EXP CACHE)
  set(CMAKE_REQUIRED_LIBRARIES "m")
  check_symbol_exists(log "math.h" HAVE_LOG)
  check_symbol_exists(exp "math.h" HAVE_EXP)
  if(HAVE_LOG AND HAVE_EXP)
    target_link_libraries(MathFunctions PRIVATE m)
  endif()
endif()

add_library(MathFunctions mysqrt.cxx)

# state that anybody linking to us needs to include the current source dir
# to find MathFunctions.h, while we don't.
target_include_directories(MathFunctions
          INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}
          )

# install rules
install(TARGETS MathFunctions DESTINATION lib)
install(FILES MathFunctions.h DESTINATION include)
```
</details>

### Part 2

<details>
<summary> Makefile </summary>

```
static_block: static_library program.o
	gcc program.o static_library.a -o static_block

dynamic_block: shared_library program.o
	gcc program.o shared_library.so -o dynamic_block

static_library: block.o
	ar qc static_library.a block.o

shared_library: block.o
	gcc -shared -o shared_library.so block.o

block.o: source/block.c
	gcc -c source/block.c -o block.o

program.o: program.c
	gcc -c program.c -o program.o
```
</details>

<details>
<summary> CMakeLists.txt </summary>

```
cmake_minimum_required(VERSION 3.10)  # CMake version check
project(Lab5)  

add_library(static_library source/block.c)
add_library(shared_library SHARED source/block.c)
add_executable(static_block program.c)
add_executable(dynamic_block program.c)
target_link_libraries(static_block static_library)
target_link_libraries(dynamic_block shared_library)
```
</details>

`ls -la` output

```
-rwxr-xr-x   1 nick-lennox  staff  49440 Feb 11 13:28 dynamic_block
-rwxr-xr-x   1 nick-lennox  staff  49520 Feb 11 13:29 static_block
```

<img src="https://i.imgur.com/UAUBe3w.png" width=750>
