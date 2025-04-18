cmake_minimum_required(VERSION 3.11) # FetchContent requires 3.11+
project(DoomCrossPlatform C)

# Include FetchContent module for downloading dependencies
include(FetchContent)

# Set C standard if needed (original DOOM is likely C89/C90)
# set(CMAKE_C_STANDARD 90)
# set(CMAKE_C_STANDARD_REQUIRED ON)

# Enable warnings similar to -Wall
if(CMAKE_COMPILER_IS_GNUCC OR CMAKE_C_COMPILER_ID MATCHES "Clang")
    add_compile_options(-Wall)
endif()

# Add preprocessor definitions from CFLAGS
# add_compile_definitions(NORMALUNIX LINUX) # Commented out - SDL provides abstraction

# Define the executable target and list source files
add_executable(doom # Using 'doom' as the target name, instead of linuxxdoom
    # Core
    doomdef.c
    doomstat.c
    dstrings.c
    info.c
    tables.c
    # System Interface (Original Linux/X11)
    i_system.c
    i_sound.c
    i_video.c
    i_net.c
    i_main.c
    # Main Loop & Game Logic
    d_main.c
    d_net.c
    d_items.c
    g_game.c
    # Menu & Misc
    m_menu.c
    m_misc.c
    m_argv.c
    m_bbox.c
    m_fixed.c
    m_swap.c
    m_cheat.c
    m_random.c
    # Automap
    am_map.c
    # Play Simulation (P_*)
    p_ceilng.c
    p_doors.c
    p_enemy.c
    p_floor.c
    p_inter.c
    p_lights.c
    p_map.c
    p_maputl.c
    p_plats.c
    p_pspr.c
    p_setup.c
    p_sight.c
    p_spec.c
    p_switch.c
    p_mobj.c
    p_telept.c
    p_tick.c
    p_saveg.c
    p_user.c
    # Renderer (R_*) - Software
    r_bsp.c
    r_data.c
    r_draw.c
    r_main.c
    r_plane.c
    r_segs.c
    r_sky.c
    r_things.c
    # WAD Data
    w_wad.c
    # Finale & Wipes
    f_finale.c
    f_wipe.c
    wi_stuff.c
    # Video (Software)
    v_video.c
    # Status Bar
    st_lib.c
    st_stuff.c
    # Heads Up Display
    hu_stuff.c
    hu_lib.c
    # Sound
    s_sound.c
    sounds.c
    # Memory Zone
    z_zone.c
)

# Fetch SDL2 dependency
FetchContent_Declare(
    SDL2
    GIT_REPOSITORY https://github.com/libsdl-org/SDL.git
    GIT_TAG release-2.30.2 # Use a specific stable release tag
)
FetchContent_MakeAvailable(SDL2)

# Link libraries
# Link SDL2 library (FetchContent provides the SDL2::SDL2 target)
# Include directories are usually handled automatically by target_link_libraries
target_link_libraries(doom PRIVATE SDL2::SDL2)

# Link other libraries from the Makefile's LIBS (-lnsl -lm)
# Note: nsl might not be needed on all modern Linux systems
# Math library (m) is often linked automatically by modern compilers,
# but explicitly adding it is safer for older codebases.
find_library(NSL_LIBRARY nsl)
if(NSL_LIBRARY)
    target_link_libraries(doom PRIVATE ${NSL_LIBRARY})
else()
    message(WARNING "libnsl not found. Networking might be affected if required.")
endif()
target_link_libraries(doom PRIVATE m)

# Set compiler flags for debug symbols (-g)
# CMake handles this automatically based on the build type (e.g., Debug, Release)
# For Debug builds (default if not specified):
# target_compile_options(doom PRIVATE -g)

# Optional: Set output directory for the executable
# set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/bin)