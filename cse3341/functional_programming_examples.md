# Functional Programming Examples

## Make list in descending order
```
Ref makeListDescending (int x)
{
  ???
}
```

## Make list in ascending order
```
/* bad */
Ref makeListAscending (int x)
{
  if (x == 0) return nil;
  return append (makeListAscending (x - 1), (x . nil));
}
```

```
/* better */
Ref makeListAscending (int pos, int length)
{
  if (pos >= length) return nil;
  return (pos + 1) . makeListAscending (pos + 1, length);
}
```

## Equality
```
int equals (Q x, Q y)
{
  if (isAtom (x) == 1)
  {
    if (isAtom (y) == 0) return 0;
    if (isNil (x) == 1) return isNil (y);
    if (isNil (y) == 1) return 0;
    if ((int) x == (int) y) return 1;
    return 0;
  }

  if (isAtom (y) == 1) return 0;

  if (equals (left ((Ref) x), left ((Ref) y)))
  {
    if (equals (right ((Ref) x), right ((Ref) y))) return 1;
    return 0;
  }
  return 0;
}
```

## Build a tree
```
Ref tree (int count)
{
  if (count == 0) return nil;
  Ref tiny = tree (count - 1);
  return tiny . tiny;
}
```