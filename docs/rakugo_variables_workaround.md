# Rakugo Variables Workaround

## Problem

Currently in RakuScript we **can't** do stuff like this:
```renpy
a = 1
b = a + 1
c = 1 - 2
d = 2 * 2
e = 2 / 2
```

## Temporary Solution

For now thanks to new features in 2.2,
we can workaround this using variable assignments:
```renpy
a = 1

# b = a + 1
b = a
b += 1

# c = 1 - 2
c = 1
c -= 2

# d = 2 * 2
d = 2
d *= 2

# e = 2 / 2
e = 1
e /= 2
```

