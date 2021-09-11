# Install Clang 9 On Ubuntu 18.04

Open a Terminal and make sure your system is updated:

```bash
sudo apt update
sudo apt upgrade
```

Next, we need to install a few prerequisites for running Clang:

```bash
sudo apt install build-essential xz-utils curl
```

Download and extract latest binary of Clang, which is 9.0.0 at the time of this writing:

```bash
curl -SL http://releases.llvm.org/9.0.0/clang+llvm-9.0.0-x86_64-linux-gnu-ubuntu-18.04.tar.xz | tar -xJC .
mv clang+llvm-9.0.0-x86_64-linux-gnu-ubuntu-18.04 clang_9.0.0
sudo mv clang_9.0.0 /usr/local
```

Next, you will need to add Clang to your system `PATH`:

```bash
export PATH=/usr/local/clang_9.0.0/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/clang_9.0.0/lib:$LD_LIBRARY_PATH
```

Let’s try to compile and run a **C++17** code that uses an if block with init-statement:

```c++
#include <iostream>

int main() {
    // if block with init-statement:
    if(int a = 5; a < 8) {
        std::cout << "Local variable a is < 8\n";
    } else {
        std::cout << "Local variable a is >= 8\n";
    }
    return 0;
}
```

Save the above code in a file named **test.cpp** and compile it with:

```bash
clang++ -std=c++17 -stdlib=libc++ -Wall -pedantic test.cpp -o test
```

This is what I see on my machine:

```bash
clang++ -std=c++17 -stdlib=libc++ -Wall -pedantic test.cpp -o test
./test
Local variable a is < 8
```

Good luck!
