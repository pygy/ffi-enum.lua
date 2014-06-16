# ffi-enum

Syntactic sugar for enum-like FFI structs. MIT Licensed.

```Lua
fe = require"ffi-enum"

e = fe.enum[[
  FOO, BAR, /* comments are */
  BAZ = 7,  // parsed properly 
  QUX,
  QUUX = 0xc
]]

assert(e.FOO == 0 and e.BAR == 1
   and e.BAZ == 7 and e.QUX == 8
   and e.QUUX == 12)

-- note that leading spaces are allowed for indenting.

d = fe.define[[
  #define foo 0 
  #define bar 1
  #define baz 5
  #define qux 6
  #define quux 0xFe
]]

assert(d.FOO == 0 and d.BAR == 1
   and d.BAZ == 7 and d.QUX == 8
   and d.QUUX == 12)
```

In both cases, the code is compiled to

```Lua
ffi.new[[
struct{
  static const int FOO = 0;
  static const int BAR = 1;
  static const int BAZ = 7;
  static const int QUX = 8;
  static const int QUUX = 12;
}
]]
```

The compiler treats the fields as constants, which enables 
optimizations not possible with Lua table fields.

Please note that these are not seen as enums on the C side.
