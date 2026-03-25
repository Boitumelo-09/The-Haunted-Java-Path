# 💀 Panama (FFM) + C++ Cheat Sheet

## ⚙️ Project Structure
```
C:\dev\panama-demo\
├── cpp\
├── headers\
├── java\
└── build\
```

---

## 🔥 1. Compile C++ → DLL (x64 ONLY)
Open: **x64 Native Tools Command Prompt**

```
cd C:\dev\panama-demo
cl /LD cpp\yourfile.cpp /Fe:build\yourlib.dll
```

Check:
```
dir build
```

---

## 🔥 2. Generate Bindings (jextract)
```
jextract -I headers -t <pkg> -d java headers\file.h
```

---

## 🔥 3. IntelliJ Setup
- Copy `java/` → `src/`
- Load DLL:

```java
System.load("C:\\dev\\panama-demo\\build\\yourlib.dll");
```

VM Options:
```
--enable-native-access=ALL-UNNAMED
```

---

## 🔥 4. Call Native Function
```java
Linker linker = Linker.nativeLinker();
SymbolLookup lib = SymbolLookup.loaderLookup();

MethodHandle fn = linker.downcallHandle(
    lib.find("functionName").get(),
    FunctionDescriptor.of(...)
);
```

---

## 🔥 5. Pass String
```java
try (Arena arena = Arena.ofConfined()) {
    MemorySegment str = arena.allocateUtf8String("Hello");
    fn.invoke(str);
}
```

---

## 🔧 Debug
Check Java:
```
java -version
```

Check DLL:
```
dumpbin /headers build\yourlib.dll
```

Check jextract:
```
jextract --version
```

---

## 💣 Common Errors

**DLL not found**
→ Use full path with `System.load()`

**machine code=0x14c**
→ Wrong architecture → rebuild x64

**function not found**
→ Add `extern "C"`

**restricted warning**
→ Add VM option

---

## 🧠 Flow
```
WRITE → BUILD → EXTRACT → LOAD → CALL
```
