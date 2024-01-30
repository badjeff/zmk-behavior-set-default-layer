zephyr_library()

if ((NOT CONFIG_ZMK_SPLIT) OR CONFIG_ZMK_SPLIT_ROLE_CENTRAL)

  #---
  # add a pointer to "static uint8_t _zmk_keymap_layer_default = 0;" in src/keymap.c
  # to make it be accessible via "extern"
  #---
  file(READ   ${APPLICATION_SOURCE_DIR}/src/keymap.c CONTENTS)
  if(NOT CONTENTS MATCHES "_ptr_zmk_keymap_layer_default")
    set(PATCH "uint8_t *_ptr_zmk_keymap_layer_default = &_zmk_keymap_layer_default;\n")
    file(APPEND ${APPLICATION_SOURCE_DIR}/src/keymap.c "${PATCH}")
  endif()

  zephyr_library_sources_ifdef(CONFIG_ZMK_BEHAVIOR_SET_DEFAULT_LAYER src/behavior_set_default_layer.c)
  zephyr_include_directories(${APPLICATION_SOURCE_DIR}/include)

endif()
