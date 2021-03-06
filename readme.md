How to call a function from a Windows 64-bit DLL with Panama
---

This simple example demonstrates how to access a function from a 64-bit
Windows DLL with the Foreign Linker API.

A great collection of Foreign Linker API usage examples can be found in the [sundararajana/panama-jextract-samples](https://github.com/sundararajana/panama-jextract-samples) repository.

# Compile the DLL

One way to compile the DLL is to use Visual Studio C++.
The [community edition of Visual Studio C++](https://visualstudio.microsoft.com/de/vs/features/cplusplus/) is sufficient for this.

Open the Visual Studio C++ solution in the `helloworld_dll` folder and 
build the solution with target `x64`. After building the project copy the resulting
`helloworld.dll` from the `helloworld_dll/x64/Debug` folder to project root.

# Setup Panama JDK
```
set JAVA_HOME=c:\Users\tom\dev\tools\java\jdk-16-panama
set PATH=%JAVA_HOME%\bin;%PATH%
```

# Generate Foreign Wrapper Classes with jextract

`jextract` analyses given c-header files and generates bootstrap and wrapper classes 
that can be used by Java code to use exported symbols (e.g. calling functions) from a DLL.

We use the following command to generate our wrapper classes:

```
jextract -d target/classes ^
         -t hello ^
         -lhelloworld ^
         helloworld_dll/helloworld.h
```

# Compile Java
```
javac -cp target/classes ^
      -d target\classes ^
      src\main\java\HelloPanama.java
```

# Run the Example App
```
java -cp target\classes ^
     -Dforeign.restricted=permit ^
     --add-modules jdk.incubator.foreign ^
     HelloPanama
```

# Output
```
WARNING: Using incubator modules: jdk.incubator.foreign
Hello World from DLL!
```