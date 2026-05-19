# ESP-IDF component wrapper for moononournation/Arduino_GFX.
#
# This isn't in upstream — it's added on the `idf-port` branch of this
# fork so the library can be consumed as a regular IDF component
# (REQUIRES Arduino_GFX from sibling CMakeLists). Once upstream gains
# native IDF support, this file can be deleted and the fork retired.
#
# Globs every .cpp in src/ (and its subdirs canvas/, databus/, display/,
# font/) so new files added by upstream are picked up without editing
# this file. Header-only utilities are pulled in by `INCLUDE_DIRS src`.
file(GLOB_RECURSE ARDUINO_GFX_SRCS
    "src/*.cpp"
)

idf_component_register(
    SRCS ${ARDUINO_GFX_SRCS}
    INCLUDE_DIRS "src"
    REQUIRES
        arduino   # Arduino-style API the library is built on
        esp_lcd   # esp_lcd_panel_interface.h, esp_lcd_panel_rgb.h,
                  # esp_lcd_panel_ops.h (used by Arduino_ESP32LCD8 /
                  # Arduino_ESP32RGBPanel etc.)
)

# Suppress two -Werror flavors that the IDF default flags promote
# but upstream Arduino_GFX trips on the ESP-IDF v5.5 toolchain:
#   - overloaded-virtual: Arduino_GFX::flush(bool) overload hides
#     Print::flush() from arduino-esp32's Stream/Print stack.
#   - format-truncation / unused-* are already on Wno-error by default.
target_compile_options(${COMPONENT_LIB} PRIVATE
    -Wno-error=overloaded-virtual
    -Wno-error=missing-field-initializers
)
