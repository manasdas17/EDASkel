find_package( Qt5Widgets REQUIRED )

# here's where we handle the Qt "moc" process
# have to make it a bit more explicit than in a qmake-based flow
QT5_WRAP_CPP(DV_MOC_SRC ../gui/designview.h)

if( MSVC )
  # prevents linker errors (arg, default_line_length)
  add_definitions( -DBOOST_PROGRAM_OPTIONS_DYN_LINK )
endif()

add_executable( spef load_spef.cpp )

include_directories(${TCL_INCLUDE_PATH})

if( TCL_FOUND )
  add_executable( sv simple_viewer.cpp ${DV_MOC_SRC})
  qt5_use_modules( sv Widgets )
  target_link_libraries( sv lefdef ${Boost_LIBRARIES} qttclnotifier ${TCL_LIBRARY} )
endif()

