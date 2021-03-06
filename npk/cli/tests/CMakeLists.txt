include_directories( ${PROJECT_SOURCE_DIR}/libnpk/include )

file( GLOB ITEMS RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} "*.cpp" )
create_test_sourcelist( TESTS tests.cpp ${ITEMS} )

add_executable( npkTests ${TESTS} )
target_link_libraries( npkTests libnpkStatic )
remove( TESTS tests.cpp )

# push test cases into the target
foreach( test ${TESTS} )
	get_filename_component( TName ${test} NAME_WE )
	add_test( ${TName} npkTests ${TName} )
endforeach( test )

# copy test resources into test directory
file( GLOB files "${PROJECT_SOURCE_DIR}/npk/common/testres/[^.]*" )
foreach( file ${files} )
	add_custom_command( 
		TARGET npkTests
		POST_BUILD
		COMMAND ${CMAKE_COMMAND} ARGS -E copy "${file}" "${CMAKE_CURRENT_BINARY_DIR}/"
		)
endforeach( file ${TEST_RESOURCES} )
