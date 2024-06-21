## library code (problems / discussion)
- the community has done a great job with including possible checks on things like sections and what not and locking certain privs, however this can all be bypassed by throwing the exec in IDA.
- why is it easy? well for starters lets be honest. yes, the protection works its not fake. but, they have certain things like the snippet below which make it useless.

## example from keyauth lib
```cpp
if ( check_section_integrity( XorStr( ".text" ).c_str( ), false ) )
{
    error("check_section_integrity() failed, don't tamper with the program.");
}
```
