# READM.md <!-- omit in toc -->

- [🔗 Create system link to allow MSYS UCRT64 to recognize c:\\vcpkg](#-create-system-link-to-allow-msys-ucrt64-to-recognize-cvcpkg)
- [🧬 Make VCPKG\_ROOT persistent for MSYS UCRT64 bash](#-make-vcpkg_root-persistent-for-msys-ucrt64-bash)
- [🔨 Configuration and builds](#-configuration-and-builds)
  - [ucrt64 gcc](#ucrt64-gcc)
  - [UCT64 bash and ninja preset, configure and build](#uct64-bash-and-ninja-preset-configure-and-build)
  - [ucrt64 bash and ninja configure without preset](#ucrt64-bash-and-ninja-configure-without-preset)
  - [Developer PowerShell](#developer-powershell)
- [🛤️ Add PowerShell to MSYS2 $PATH](#️-add-powershell-to-msys2-path)
- [📚 Common Windows Libraries](#-common-windows-libraries)
- [🧳 VCPKG\_APPLOCAL\_DEPS within UCRT64 CMakePresets](#-vcpkg_applocal_deps-within-ucrt64-cmakepresets)
- [🧠➕ Add VCPKG to terminal path](#-add-vcpkg-to-terminal-path)
  - [▶️ Get path to vcpkg](#️-get-path-to-vcpkg)
  - [CMAKE\_RUNTIME\_OUTPUT\_DIRECTORY vs install()](#cmake_runtime_output_directory-vs-install)
- [🎯 Target Installation](#-target-installation)
  - [ucrt64-gcc install](#ucrt64-gcc-install)
  - [MSVC install](#msvc-install)
  - [CMakeLists.txt installation code example](#cmakeliststxt-installation-code-example)
- [🐙 Github identity](#-github-identity)
- [CMake generic configure, build, and install](#cmake-generic-configure-build-and-install)
- [CMAKE\_TOOLCHAIN\_FILE](#cmake_toolchain_file)
  - [🔧 vcpkg.cmake](#-vcpkgcmake)
  - [📌 Role of `CMAKE_FIND_PACKAGE_PREFER_CONFIG`](#-role-of-cmake_find_package_prefer_config)
- [🧠 VS Code Markdown All In One extension](#-vs-code-markdown-all-in-one-extension)
- [📄 Editing \& Structure](#-editing--structure)
  - [Common Shortcuts](#common-shortcuts)
- [🧭 Navigation \& Headings](#-navigation--headings)
  - [Outline \& Commands](#outline--commands)
- [🔄 Preview \& Refresh](#-preview--refresh)
  - [Preview Behavior](#preview-behavior)
- [🧩 Markdown All in One Features](#-markdown-all-in-one-features)
  - [Extension-Specific Shortcuts](#extension-specific-shortcuts)
- [🧠 Style Tips for Reproducible Guides](#-style-tips-for-reproducible-guides)
  - [Visual Grammar](#visual-grammar)
- [📁 Suggested Directory Structure](#-suggested-directory-structure)
  - [Workspace Layout](#workspace-layout)

## 🔗 Create system link to allow MSYS UCRT64 to recognize c:\vcpkg

from ucrt64 bash:

```bash
mkdir -p /usr/local/bin
ln -s /c/vcpkg/vcpkg.exe /usr/local/bin/vcpkg
export VCPKG_ROOT=/c/vcpkg  
```

---

## 🧬 Make VCPKG_ROOT persistent for MSYS UCRT64 bash

```Cmakepresets
echo 'export VCPKG_ROOT=/c/vcpkg' >> ~/.bashrc</br>
```

---

## 🔨 Configuration and builds

### ucrt64 gcc

```bash
rm -rf build/ucrt64-gcc
cmake --preset ucrt64-gcc
cmake --build --preset ucrt64-gcc
```

### UCT64 bash and ninja preset, configure and build

```cmake
cmake --preset ucrt64-ninja
cmake --build --preset ucrt64-ninja
```

### ucrt64 bash and ninja configure without preset

```bash
cmake -S . -B build \
  -G Ninja \
  -DCMAKE_BUILD_TYPE=Release \
  -DCMAKE_INSTALL_PREFIX=/ucrt64
```

> build: `cmake --build build`

### Developer PowerShell

```Powershell
cmake --preset MSVC-manifest
cmake --build --preset MSVC-manifest
```

---

## 🛤️ Add PowerShell to MSYS2 $PATH  

```bash
export PATH="/c/Windows/System32/WindowsPowerShell/v1.0:$PATH"
```

---

## 📚 Common Windows Libraries

| Library      | Purpose                                                |
|--------------|--------------------------------------------------------|
| `kernel32`   | Core Windows API: memory, processes, threads, file I/O |
| `user32`     | GUI: windows, messages, input, dialogs                 |
| `gdi32`      | Graphics: drawing, fonts, bitmaps                      |
| `winmm`      | Multimedia: timers, sound, joystick                    |
| `shell32`    | Shell operations: file dialogs, launching apps         |
| `ole32`      | COM object support                                     |
| `oleaut32`   | Automation support for COM                             |
| `uuid`       | GUID generation and COM identifiers                    |

Windows libraries location: C:\Program Files (x86)\Windows Kits\10\Lib\10.0.26100.0\um\x64

---

## 🧳 VCPKG_APPLOCAL_DEPS within UCRT64 CMakePresets

VCPKG_APPLOCAL_DEPS controls whether vcpkg runs a post-build PowerShell script applocal.ps1 to copy runtime dependencies next to your binary. MSYS2 does not recognize PowerShell.

applocal.ps1 is a PowerShell script used by vcpkg to copy runtime dependencies (like DLLs) next to your built executable after a successful build. It’s part of vcpkg’s AppLocal deployment strategy, which ensures your binary can run without needing global installs or PATH tweaks.

| Platform               | VCPKG_APPLOCAL_DEPS |
|------------------------|---------------------|
| MSVC (PowerShell OK)   | "ON" (default)      |
| MSYS2 UCRT64 (bash)    | "OFF"               |

---

## 🧠➕ Add VCPKG to terminal path

PowerShell

```Powershell
env:PATH += ";C:\vcpkg"
```

bash

```bash
export PATH="$PATH:/c/vcpkg"
```

### ▶️ Get path to vcpkg

```Powershell
Get-Command vcpkg
```

---

### CMAKE_RUNTIME_OUTPUT_DIRECTORY vs install()

| Setting                        | Purpose                                                   |
| ------------------------------ | ----------------------------------------------------------|
| CMAKE_RUNTIME_OUTPUT_DIRECTORY | Where your compiler drops the binary                      |
|                                | to control where executables land                         |
| install()                      | Where you want to ship or stage the binary                |
|                                | Unnecessary during iterative builds. Use for deployment   |

| Variable                       | Description                                                          |
| ------------------------------ | -------------------------------------------------------------------- |
| CMAKE_RUNTIME_OUTPUT_DIRECTORY | Where executables are placed during the build (e.g., `build/bin/`)   |
| CMAKE_INSTALL_PREFIX           | Where files are copied during the install step (e.g., `/opt/myapp/`) |

---

## 🎯 Target Installation

vs code taskbar configuration only does configurations and build, NOT install. Install must be initiated via terminal, bash (gcc), or developer PowerShell (MSVC). Outputs just outputs but install installs, in order to maintain separation.

### ucrt64-gcc install

```bash
cmake --install build/ucrt64-gcc
```

### MSVC install

```Powershell
cmake --install build/release --config Release
```

### CMakeLists.txt installation code example

```cmake
install(TARGETS ${PROJECT_TARGET}
        RUNTIME DESTINATION bin
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib/static
)
```

---

## 🐙 Github identity

Your Identity
The first thing you should do when you install Git is to set your user name and email address. This is important because every Git commit uses this information, and it’s immutably baked into the commits you start creating:

```git
git config --global user.name "TotalEnnui"
git config --global user.email aprpkp@gmail.com
```

---

## CMake generic configure, build, and install

```PowerShell
cmake -S . -B ./build      # Configure: generate build system files
cmake --build ./build      # Build: compile the project
cmake --install ./build    # Install: copy built artifacts to install location
```

| Argument   | Description                         |
|------------|-------------------------------------|
| -S         | Source directory is current folder  |
| -B ./build | binary (build) directory is ./build |

- *Uses default generator (e.g., Ninja or MSVC depending on environment)*
- *No presets, no cache variables unless passed manually*
- *CMake ignores CMakePresets.json*
- *Manual configuration that bypasses the preset system entirely*

---

## CMAKE_TOOLCHAIN_FILE

vcpkg has a dedicated toolchain file (vcpkg.cmake) that redirects find_package calls to the correct vcpkg locations. Without it, CMake won’t use vcpkg properly.  CMAKE_TOOLCHAIN_FILE points to the location of vcpkg.cmake.

### 🔧 vcpkg.cmake

This is the toolchain file provided by **vcpkg**. It overrides CMake’s default search behavior so that:

- `find_package()` and `find_library()` calls are redirected to the vcpkg-installed libraries  
- It injects the correct include paths, library directories, and package configuration files  

**Without it:**  
CMake will fall back to system paths (`/usr/lib`, `C:\Program Files`, etc.), which breaks reproducibility and portability.

### 📌 Role of `CMAKE_FIND_PACKAGE_PREFER_CONFIG`

- **Default behavior**:  
  CMake tries **Module mode first** (`FindFoo.cmake`) and only then **Config mode**.

- **With vcpkg**:  
  You usually want **Config mode first**, because:
  - vcpkg ports are designed to export modern CMake targets (`Foo::Foo`).
  - Built‑in `FindFoo.cmake` modules may point to system libraries instead of your vcpkg‑managed ones.

- **Solution**:  
  Set the variable to prefer Config mode:

```cmake
  set(CMAKE_FIND_PACKAGE_PREFER_CONFIG ON)
```

---

## 🧠 VS Code Markdown All In One extension

> A reproducible reference for editing, previewing, and navigating Markdown in Visual Studio Code with the **Markdown All in One** extension.

---

## 📄 Editing & Structure

### Common Shortcuts

| Shortcut            | Action                                      |
|---------------------|---------------------------------------------|
| `Ctrl+Shift+V`      | Open Markdown preview                       |
| `Ctrl+K V`          | Open preview to the side (split view)       |
| `Ctrl+B`            | Toggle bold (`**text**`)                    |
| `Ctrl+I`            | Toggle italic (`*text*`)                    |
| `Ctrl+Shift+[` / `]`| Fold/unfold heading sections                |
| `Alt+Shift+F`       | Format document (Markdown All in One)       |

---

## 🧭 Navigation & Headings

### Outline & Commands

| Shortcut            | Action                                      |
|---------------------|---------------------------------------------|
| `Ctrl+Shift+O`      | Show outline of headings                    |
| `Ctrl+Shift+P`      | Command palette (type “Markdown: …”)        |
| `Ctrl+K Ctrl+S`     | Show all keyboard shortcuts                 |

---

## 🔄 Preview & Refresh

### Preview Behavior

| Shortcut            | Action                                      |
|---------------------|---------------------------------------------|
| `Ctrl+Shift+V`      | Open preview tab                            |
| `Ctrl+K V`          | Open preview to the side                    |
| `Ctrl+W`            | Close preview tab                           |
| `Ctrl+S`            | Save file (forces preview refresh)          |

> Tip: Markdown preview updates automatically, but closing/reopening the tab ensures a full refresh.

---

## 🧩 Markdown All in One Features

### Extension-Specific Shortcuts

| Feature             | Shortcut / Command                          |
|---------------------|---------------------------------------------|
| Toggle list         | `Ctrl+L`                                    |
| Toggle checkbox     | `Ctrl+Shift+C`                              |
| Insert table        | `Ctrl+T`                                    |
| Auto preview        | Enabled by default                          |
| Format on save      | Can be enabled in settings                  |

---

## 🧠 Style Tips for Reproducible Guides

### Visual Grammar

- Use `---`, `***`, or `___` for structural breaks between major sections
- Use `**This text is bold**`, `__This text is also bold__`, **BOLD**
- Use `*This text is italic*`, `_This text is also italic_`, *italic*
- Stick to sentence case for headings
- Prefer reference-style links for internal navigation
- Avoid HTML comments unless absolutely necessary

---

## 📁 Suggested Directory Structure

### Workspace Layout

```text
|- docs/        # Markdown guides and cheat sheets
|- templates/   # Reusable scaffolds for new projects
|- assets/      # Images or diagrams for documentation
```
