`make2dot` generates the DAG of makefile dependencies (targets and
prerequisites) in DOT format.

```bash
$ cat test.mk
a: b g i

b: c d

c: e

e: d f

g: h

.PHONY: a b c d e f g h i

$ ./make2dot -f test.mk
digraph make2dot {
    a -> b;
    b -> c;
    c -> e;
    e -> d;
    e -> f;
    a -> g;
    g -> h;
    a -> i;
}
```

![test.png](https://g.gravizo.com/g?digraph%20make2dot%20%7B%0A%20%20%20%20a%20-%3E%20b%3B%0A%20%20%20%20b%20-%3E%20c%3B%0A%20%20%20%20c%20-%3E%20e%3B%0A%20%20%20%20e%20-%3E%20d%3B%0A%20%20%20%20e%20-%3E%20f%3B%0A%20%20%20%20a%20-%3E%20g%3B%0A%20%20%20%20g%20-%3E%20h%3B%0A%20%20%20%20a%20-%3E%20i%3B%0A%7D%0A)

"Debug mode" is activated by setting `DEBUG` environment variable:

```bash
$ DEBUG=1 ./make2dot -f test.mk
a  # 0 "a"
  b  # 1 "b"
    c  # 2 "c"
      e  # 3 "e"
        d  # 4 "d"
        f  # 4 "f"
  g  # 1 "g"
    h  # 2 "h"
  i  # 1 "i"
```

If `make2dot` is in the $PATH, you should be able to see the DAG of
Makefile dependencies with commands like these:

```bash
dot -Tpng -o /tmp/1.png <(make2dot)  # expects `Makefile' in current dir
open /tmp/1.png
```
