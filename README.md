# C++ Learning curve

## About this repository

This repository will document my C++ learning journey. It will also serve as document to look back and see how far I have come.

## Adding library in CLion project

Download and build library, make sure you know where .dll will be.

1. Build Crypto++ library
    - Building instructions for [Crypto++](https://cryptopp.com/) were inside zip file I downloaded
    - After downloading zip file, there was Install.txt file with instructions. Because I never worked with C++ libraries, I didn't know how to follow them.
    - After few hours of research about things mentioned in file, I realized i need to run commands for installing... and that is it. Commands were:
        - `make`
        - `make test`
        - `make install` (because i am on windows)
    - The issue with running `make` command is, I don't have it, but I have MinGW installed with MSYS2.
    - Turns out MSYS2 will install `g++`, `make` after following this guide [MSYS2 Getting Started](https://www.msys2.org/).
    - This allowed me to run commands required in Install.txt and this step wasn't obstacle any more.
2. Linking Crypto++ library
    - I needed library code in project to be able to import header files.
    - After adding code to test if I can use the library, I was getting error that reference to CryptoPP namespace is undefined.
    - Linking failed...
    - After following instructions for [adding static library to CLion](https://www.jetbrains.com/help/clion/quick-cmake-tutorial.html#static-libs) i learned that linking failed because `.dll` wasn't found.
    - After search I found out that CMake can't find `.dll` so I searched where is `usr/local` on my machine and found [this](https://stackoverflow.com/questions/38230041/what-is-the-equivalent-of-usr-lib-on-windows).
    - After going to `%windir%\system32` i found `libcryptopp.dll`.
    - After that I just copied it to CLion project and linked it as instructions ordered, then code worked.

After building library and linking it in CLion, i used the following code to verify that it works.

```cpp
#include "cryptlib.h"
#include "sha.h"
#include <iostream>

int main (int argc, char* argv[])
{
    using namespace CryptoPP;

    SHA256 hash;	
    std::cout << "Name: " << hash.AlgorithmName() << std::endl;
    std::cout << "Digest size: " << hash.DigestSize() << std::endl;
    std::cout << "Block size: " << hash.BlockSize() << std::endl;

    return 0; 
}
```
