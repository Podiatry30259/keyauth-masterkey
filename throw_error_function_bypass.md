## lets cut the yap, and just go into the case, this function error(std::string) can be simply bypassed by doing this.
- go to library code and follow those instructions you will see that it is super easy to get access to this function otherwise look for system ( import ) xref until you find the function that matches.
- all of the code is public so it makes this very easy.

```cpp
void error(std::string)
48 89 5C 24 10 57 48 81 EC A0 00 00 00 48 8B 05 ?? ?? ?? ?? 48 33 C4 48 89 84 24 98 00 00 00 48 8B F9 48 89 8C 24 90 00 00 00 48 8B 49 10 48 B8 FF FF FF FF FF FF FF 7F 48 2B C1 48 83 F8 2D 0F 82 06 02 00 00 48 8B C7 48 83 7F 18 0F 76 03 48 8B 07
```
