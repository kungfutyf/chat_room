cmake_minimum_required(VERSION 3.4)

project(altest)
set(targetname "altest")
set(SRC_LIST_ALL)
set(INC_LIST_ALL)

function(add_cpp cpp_dir)
set(TMP_FILE)
list(APPEND TMP_FILE ${cpp_dir}/*.cpp)
list(APPEND TMP_FILE ${cpp_dir}/*.cxx)
list(APPEND TMP_FILE ${cpp_dir}/*.hpp)
list(APPEND TMP_FILE ${cpp_dir}/*.cc)
file(GLOB_RECURSE TMP_FILE ${TMP_FILE})
#list(APPEND SRC_LIST_ALL ${TMP_FILE})
set(SRC_LIST_ALL "${TMP_FILE}" PARENT_SCOPE)
endfunction()

function(add_inc inc_dir)
set(TMP_FILE)
#list(APPEND TMP_FILE ${inc_dir}/*.hpp)
list(APPEND TMP_FILE ${inc_dir})
#file(GLOB_RECURSE TMP_FILE ${TMP_FILE})
#list(APPEND INC_LIST_ALL ${TMP_FILE})
set(INC_LIST_ALL "${TMP_FILE}" PARENT_SCOPE)
endfunction() 

add_cpp(${CMAKE_SOURCE_DIR}/src)
add_inc(${CMAKE_SOURCE_DIR}/src/include)

message(${SRC_LIST_ALL})
message(${INC_LIST_ALL})

include_directories(
	${INC_LIST_ALL}
	)
add_library(code STATIC ${SRC_LIST_ALL})
target_compile_options(
    code
    PUBLIC
    -std=c++11
    -O0
    -g
    -Werror=return-type
)

file(GLOB_RECURSE DEMO src/example/*.cpp)

foreach(filee ${DEMO})
	get_filename_component(targetname ${filee} NAME_WE)
	add_executable(${targetname} ${filee})
	target_include_directories(${targetname} PUBLIC ${INC_LIST_ALL})
	target_link_libraries(${targetname} PUBLIC code)

target_compile_options(
    ${targetname}
    PUBLIC
    -std=c++11
    -O0
    -g
)
endforeach(filee)
