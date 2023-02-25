# pyi_macOS

Check-up for so exported symbol on macOS to fix the issue of [dm-reverb](https://github.com/llv22/dm_reverb_macOS).

Reference:

* [[BUG]: crash on macos 11.6 with python 3.9.1 #3364](https://github.com/pybind/pybind11/issues/3364)

Solution:

```bash
clang++ -g -Wall -shared -std=c++11 -fPIC -I../include binding.cc -I/Users/llv23/opt/miniconda3/include/python3.8 -Wno-unused-result -Wsign-compare -Wunreachable-code -DNDEBUG -g -fwrapv -O3 -Wall -Wstrict-prototypes -arch x86_64 -L/Users/llv23/opt/miniconda3/lib/python3.8/config-3.8-darwin -ldl -framework CoreFoundation -undefined dynamic_lookup -o example.so
clang++ -g -Wall -shared -std=c++11 -fPIC -I../include binding.cc $(python3.8-config --cflags --ldflags) -undefined dynamic_lookup -o libpybind.so
clang++ -g -Wall -shared -std=c++11 -fPIC -I../include binding.cc $(python3-config --cflags --ldflags) -undefined dynamic_lookup -o libexample3.7.so
```

The final root cause of the issue below during loading reverb/pybind11 library:

```bash
Fatal Python error: _PyInterpreterState_Get(): no current thread state
Python runtime state: unknown

Abort trap: 6
```

is due to **the binding with libpython3.8.dylib**, which has already been finished by pybind11 framework itself, also refer to [dm_reverb on macOS](https://github.com/llv22/dm_reverb_macOS).
