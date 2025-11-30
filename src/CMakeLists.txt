set(sources
    ./Application.cpp
    ./NutritionTracker.cpp
    ./imgui_combo_autoselect.cpp
)

set(headers
    ./Utils.h
    ./Application.h
    ./NutritionTracker.h
    ./imgui_combo_autoselect.h
)

# The OpenGL library's name differs across operating systems.
set(OpenGL
    "$<$<PLATFORM_ID:Linux>:GL>" 
    "$<$<PLATFORM_ID:Windows>:opengl32>"
    "$<$<PLATFORM_ID:Darwin>:-framework OpenGL>"
)

add_executable(main WIN32 ${sources} ${headers})
target_link_libraries(main PRIVATE
    project_options
    # project_warnings
    fmt::fmt
    range-v3::range-v3
    nlohmann_json::nlohmann_json
    SDL2::SDL2
    SDL2::SDL2main
    imgui
    ${OpenGL}
)

# Package the project
package_project(TARGETS main)
