## Лабораторная работа 4

Переделан CMakeLists.txt:
```sh
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(cmake)

add_subdirectory(formatter_lib)
add_subdirectory(formatter_ex_lib)
add_subdirectory(hello_world_application)
add_subdirectory(solver_lib)
add_subdirectory(solver_application)
```

Добавленно .github/workflows/build.yml:

```sh
name: Formatter

on:
  push:
    branches: 
    - main

  workflow_dispatch:

jobs:
  Build-library:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2

      - name: Build formatter_lib
        shell: bash
        run: |
          cd formatter_lib
          mkdir build_ && cd build_
          cmake ..
          cmake --build .
          
      - name: Build formatter_ex_lib
        shell: bash
        run: |
          cd formatter_ex_lib
          mkdir build_ && cd build_
          cmake ..
          cmake --build .
      - name: Build solver_lib
        shell: bash
        run: |
          cd solver_lib
          mkdir build_ && cd build_
          cmake ..
          cmake --build .
  
  Hello-world:
    runs-on: ubuntu-latest
    needs: [ Build-library ]
    
    steps:
      - uses: actions/checkout@v2
      
      - name: Build Hello_world
        shell: bash
        run: |
          mkdir build1 && cd build1
          cmake ..
          cmake --build .
  
      - name: writer
        shell: bash
        run: |
          cd build1/hello_world_application
          ./hell
  Build-solver:
    runs-on: ubuntu-latest
    needs: [ Build-library ]
    
    steps:
      - uses: actions/checkout@v2
      
      - name: Build solver_application
        shell: bash
        run: |
          mkdir build2 && cd build2
          cmake ..
          cmake --build .
          
      - name: writer
        shell: bash
        run: |
          cd build2/solver_application
          ./solve 1 1 1
```
