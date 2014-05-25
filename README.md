# ffi-enum

Syntactic sugar for enum-like FFI structs. MIT Licensed.

```Lua
enum = require"ffi-enum"

e = enum[[
  FOO, BAR, /* comments are */
  BAZ = 7,  // parsed properly 
  QUX,
  QUUX = 0xc
]]

assert(e.FOO == 0 and e.BAR == 1
   and e.BAZ == 7 and e.QUX == 8
   and e.QUUX == 12)
```

The code above is compiled to

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
