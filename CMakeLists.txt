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
)
