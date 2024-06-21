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

## going into it
- looks secure? you would be wrong. when in IDA you may be like oh you can just search for .text but they are hashing these strings at build time so its a little harder not really though.
- you could simply bypass the xor str and be on your way but they mess up with the "error" function again this has a major vuln because of these calls.
- you can notice "check_section_integrity() failed, don't tamper with the program." a string that is unprotected or gaurded. because of this we can xref it to find the function that it belongs to.

## parent function
```cpp
void modify()
{
    // code submitted in pull request from https://github.com/Roblox932
    check_section_integrity( XorStr( ".text" ).c_str( ), true );

    while (true)
    {
        if ( check_section_integrity( XorStr( ".text" ).c_str( ), false ) )
        {
            error("check_section_integrity() failed, don't tamper with the program.");
        }
        // code submitted in pull request from https://github.com/sbtoonz, authored by KeePassXC https://github.com/keepassxreboot/keepassxc/blob/dab7047113c4ad4ffead944d5c4ebfb648c1d0b0/src/core/Bootstrap.cpp#L121
        if(!LockMemAccess())
        {
            error("LockMemAccess() failed, don't tamper with the program.");
        }
        // code submitted in pull request from https://github.com/BINM7MD
        if (Function_Address == NULL) {
            Function_Address = FindPattern(PBYTE("\x48\x89\x74\x24\x00\x57\x48\x81\xec\x00\x00\x00\x00\x49\x8b\xf0"), XorStr("xxxx?xxxx????xxx").c_str()) - 0x5;
        }
        BYTE Instruction = *(BYTE*)Function_Address;

        if ((DWORD64)Instruction == 0xE9) {
            error("Pattern checksum failed, don't tamper with the program.");
        }
        Sleep(50);
    }
}
```
- in our case it belongs to this function.
- now not only can we hook this function but now without even having to try we can find "check_section_integrity" "LockMemAccess" "FindPattern"
