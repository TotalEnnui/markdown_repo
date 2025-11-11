# vcpkg setup & configs

## Linux setup

When using vcpkg, ideally it should automatically provide dependencies without much manual configuration. However, CMake needs to be explicitly told where to find vcpkg-managed packages. Here’s why each step is necessary:

1. CMAKE_TOOLCHAIN_FILE:
• vcpkg has a dedicated toolchain file (vcpkg.cmake) that redirects find_package calls to the correct vcpkg locations. Without it, CMake won’t use vcpkg properly.
2. CMAKE_PREFIX_PATH:
• Some packages don’t get detected just by the toolchain file alone, so CMAKE_PREFIX_PATH ensures CMake looks in /home/adrian/vcpkg/installed/x64-linux.
3. fmt_DIR:

• While CMAKE_PREFIX_PATH should theoretically be enough, sometimes specifying fmt_DIR directly makes find_package(fmt CONFIG REQUIRED) work without ambiguity.
Why Isn’t This Fully Automatic?

Unlike package managers like apt or brew, vcpkg works in a per-project context rather than globally. This means:
• CMake doesn’t automatically assume you’re using vcpkg.
• Different versions of dependencies might exist across projects.
• It avoids conflicts with system-installed libraries.
What Could Make This Smoother?
• If you set VCPKG_ROOT globally:
sh

export VCPKG_ROOT=/home/adrian/vcpkg

This would reduce redundant path settings.
• If your presets included:

```json
{
  "cacheVariables": {
    "CMAKE_FIND_PACKAGE_PREFER_CONFIG": "ON"
  }
}
```

This prioritizes vcpkg's config files over system-wide installations.
Final Thoughts
It should be easier, and in most cases, just setting CMAKE_TOOLCHAIN_FILE is enough. But since fmt wasn't being detected, layering CMAKE_PREFIX_PATH and fmt_DIR ensured CMake worked correctly. Now that you’ve got this locked down, future package integrations should be much smoother!
