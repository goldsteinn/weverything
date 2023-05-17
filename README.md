# Weverything For GCC/Clang

## What?
- **Enables every warning for given GCC/Clang version + language**
- **Version aware**
    - Won't set/unset warning that is unsupported
- **Contains list of all existing warnings**
    - Easy to disable, just uncomment a line

## Usage
```
# Include this in a CMakeLists.txt
include(warn-everything.cmake)
warn_everything(WARN_EVERYTHING_CXX WARN_EVERYTHING_C)
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${WARN_EVERYTHING_C}")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${WARN_EVERYTHING_CXX}")
```

## Disabling A Warning

#### GCC:`warn-everything-gcc.cmake`

- Enabled
```
  # All of these are enabled (not turned OFF)
  set(GCC_4_4_7_GENERIC_WARNING_FLAGS_OFF
    # ${WARN_LARGER_THAN}
    # -Wabi
    # -Waddress
    # -Waggregate-return
    # -Wall
    # -Warray-bounds
    # ...
  )
```
- Disabled
```
  # -WAddress and -Warray-bounds are disabled, the rest are still on.
  set(GCC_4_4_7_GENERIC_WARNING_FLAGS_OFF
    # ${WARN_LARGER_THAN}
    # -Wabi
     -Waddress
    # -Waggregate-return
    # -Wall
     -Warray-bounds
    # ...
  )
```

**For GCC, you should only ever need to modify '*_OFF' variables.**

### Clang:`warn-everything-clang.cmake`

- Enabled
```
  # -Weverything is set, and no '-Wno-*' are enabled.
  set(CLANG_4_0_0_C_AND_CXX_WARNING_FLAGS_ACTIVATED
    -Weverything # Keep this set
    # -Wno-CFString-literal
    # -Wno-CL4
    # -Wno-IndependentClass-attribute
    # -Wno-NSObject-attribute
    # -Wno-abi
    # ...
  )
```
- Disabled
```
  # Now supressing -WCL4 and -Wabsolute-value
  set(CLANG_4_0_0_C_AND_CXX_WARNING_FLAGS_ACTIVATED
    -Weverything # Keep this set
    # -Wno-CFString-literal
     -Wno-CL4
    # -Wno-IndependentClass-attribute
    # -Wno-NSObject-attribute
    # -Wno-abi
     -Wno-absolute-value
    # -Wno-abstract-final-class
    # -Wno-abstract-vbase-init
    # ...
  )
```

**For Clang, you should only ever need to modify '*_ACTIVATED' variables.**

## More info in header comment of `warn-everything.cmake`
