# Weverything For GCC/Clang

## What?
- **Enables every warning for given GCC/Clang version + language**
- **Version aware**
    - Won't set/unset warning that is unsupported
- **Contains list of all existing warnings**
    - Easy to disable, just uncomment a line

## Where?
- `cmake/*.cmake`

## Why?
- Sometimes it interesting to see all the warnings active. For clang
  thats not to hard because of the builtin `-Weverything` flag. For
  GCC no such flag exists. Like flags change by version, and there
  didn't seem to be any clean way to enable/disable all warnings at
  will without a billion version checks. This isn't the worlds
  cleanest thing, but its better than the alternatives I'm aware of.
- **This is probably best not used for standard builds**

## API
```
# To get warnings as space seperated string
warn_everything(WARN_EVERYTHING_CXX WARN_EVERYTHING_C)
#
# To get list of warnings
warn_everything_list(WARN_EVERYTHING_CXX WARN_EVERYTHING_C)
#
# To get warnings filtering OUT by regex match of first argument
set(RE_FILTER
  -Wsuggest-attribute=*
  -W*overflow*
)
warn_everything_list(RE_FILTER WARN_EVERYTHING_CXX WARN_EVERYTHING_C)
```

## Usage
```
# Include this in a CMakeLists.txt
include(warn-everything.cmake)
# To get warnings as space seperated string
warn_everything(WARN_EVERYTHING_CXX WARN_EVERYTHING_C)
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${WARN_EVERYTHING_C}")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${WARN_EVERYTHING_CXX}")
```

## Disabling A Warning

#### GCC:`warn-everything-gcc.cmake`

- Enabled
```
  # All of these are enabled
  set(GCC_4_4_7_GENERIC_WARNING_FLAGS_ACTIVATED
    ${WARN_LARGER_THAN}
    -Wabi
    -Waddress
    -Waggregate-return
    -Wall
    -Warray-bounds
    -Wattributes
    # ...
  )
```
- Disabled
```
  # -WAddress and -Warray-bounds are disabled, the rest are still on.
  # All of these are enabled (not turned OFF)
  set(GCC_4_4_7_GENERIC_WARNING_FLAGS_ACTIVATED
    ${WARN_LARGER_THAN}
    -Wabi
    # -Waddress
    -Waggregate-return
    -Wall
    # -Warray-bounds
    -Wattributes
    # ...
  )
```

**For GCC, you should only ever need to modify '*_ACTIVATED' variables.**

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
