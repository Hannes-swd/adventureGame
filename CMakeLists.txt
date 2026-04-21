cmake_minimum_required(VERSION 3.23)

project(Myproject CXX)

# C++ Standard setzen (z.B. C++17 oder C++20)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

include(FetchContent)

# Raylib herunterladen
FetchContent_Declare(
  raylib
  GIT_REPOSITORY https://github.com/raysan5/raylib.git
  GIT_TAG 5.5
)
FetchContent_MakeAvailable(raylib)

# Alle .cpp Dateien im src/ Ordner sammeln
file(GLOB_RECURSE SOURCES "src/*.cpp")

# Alle .h und .hpp Dateien (optional, für IDEs)
file(GLOB_RECURSE HEADERS "src/*.h" "src/*.hpp")

# Ausführbare Datei
add_executable(MyProgram ${SOURCES} ${HEADERS})

# Raylib linken
target_link_libraries(MyProgram PRIVATE raylib)

# Include-Verzeichnisse (dein eigenes src/ + Raylib intern)
target_include_directories(MyProgram PRIVATE 
    ${CMAKE_CURRENT_SOURCE_DIR}/src
    ${raylib_SOURCE_DIR}/src
)

# Assets kopieren (Texturen, Sounds, Fonts etc.)
# Hier: Alle Dateien aus assets/ nach dem Build in den Ausgabeordner kopieren
set(ASSETS_DEST ${CMAKE_CURRENT_BINARY_DIR}/assets)
file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/assets/ DESTINATION ${ASSETS_DEST})

# Dem Programm mitteilen, wo die Assets zur Laufzeit sind
target_compile_definitions(MyProgram PRIVATE 
    ASSETS_PATH="${ASSETS_DEST}"
)

# Optional: Plattformspezifische Einstellungen
if(WIN32)
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -mwindows")
endif()