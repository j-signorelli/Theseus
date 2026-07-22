# Add the relative path (from main directory) to the list of file names
# (Inspired largely from convert_filenames_to_full_paths in MFEM)
function(append_relative_paths NAMES)
   unset(tmp_names)
   foreach(name ${${NAMES}})
      set(rel_path "")
      cmake_path(RELATIVE_PATH CMAKE_CURRENT_SOURCE_DIR
                  BASE_DIRECTORY ${CMAKE_SOURCE_DIR} 
                  OUTPUT_VARIABLE rel_path)
      list(APPEND tmp_names ${rel_path}/${name})
   endforeach()
   set(${NAMES} ${tmp_names} PARENT_SCOPE)
endfunction()


set(THESEUS_SOURCES
  

)


set(THESEUS_SOURCES
  

)

append_relative_paths(THESEUS_SOURCES)
append_relative_paths(THESEUS_HEADERS)