find_package(Lua 5.1 EXACT)

set(LUA_SUPPORT_DEFAULT "ON")

if (NOT LUA_FOUND)
	set(LUA_SUPPORT_DEFAULT "OFF")
endif (NOT LUA_FOUND)

option(LUA_SUPPORT "whether to support Lua" ${LUA_SUPPORT_DEFAULT})

if(LUA_SUPPORT)
	include_directories(${LUA_INCLUDE_DIR})
	add_library(evmongoose-lua MODULE lua_mongoose.c lua_posix.c)
	set_target_properties(evmongoose-lua 
                PROPERTIES OUTPUT_NAME evmongoose 
                            PREFIX ""
                            INSTALL_RPATH "@loader_path/../.."
                )
		
    target_compile_options(evmongoose-lua PRIVATE -Wno-error-deprecated-declarations)
    target_link_libraries(evmongoose-lua evmongoose util ${LUA_LIBRARY})

    install(TARGETS evmongoose-lua
		LIBRARY DESTINATION lib/lua/${LUA_VERSION_MAJOR}.${LUA_VERSION_MINOR}
	)
endif(LUA_SUPPORT)
