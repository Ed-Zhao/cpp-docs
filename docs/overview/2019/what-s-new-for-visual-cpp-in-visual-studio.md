---
title: "What's New for C++ in Visual Studio 2019"
ms.date: "04/02/2019"
ms.technology: "cpp-ide"
ms.assetid: 8801dbdb-ca0b-491f-9e33-01618bff5ae9
author: "mikeblome"
ms.author: "mblome"
---

<!--NOTE all https:// links to docs.microsoft.com need to be converted to site-relative links prior to publishing-->

# What's New for C++ in Visual Studio 2019

Visual Studio 2019 brings many updates and fixes to the Microsoft C++ environment. We've fixed many bugs and reported issues in the compiler and tools, many submitted by customers through the [Report a Problem](/visualstudio/how-to-report-a-problem-with-visual-studio-2017) and [Provide a Suggestion](https://developercommunity.visualstudio.com/spaces/62/index.html) options under **Send Feedback**. Thank you for reporting bugs! For more information on what's new in all of Visual Studio, visit [What's new in Visual Studio](/visualstudio/ide/whats-new-visual-studio-2019).

## C++ compiler

- The `/std:c++latest` option now includes C++20 features that aren't necessarily complete, including initial support for the C++20 operator <=> ("spaceship") for three-way comparison.

- [P0941R2 - feature-test macros](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2018/p0941r2.html) is complete, with support for `__has_cpp_attribute`. Feature-test macros are supported in all Standard modes.

- [C++20 P1008R1 - prohibiting aggregates with user-declared constructors](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2018/p1008r1.pdf) is also complete.

- Enhanced support for C++17 features, plus experimental support for C++20 features such as modules and coroutines. For detailed information, see [C++ Conformance Improvements in Visual Studio 2019](../cpp-conformance-improvements.md).

- The C++ compiler switch `/Gm` is now deprecated. Consider disabling the `/Gm` switch in your build scripts if it's explicitly defined. However, you can also safely ignore the deprecation warning for `/Gm`, because it's not treated as an error when using "Treat warnings as errors" (`/WX`).

- Precompiled headers are no longer generated by default for C++ console and desktop apps.

### Codegen, security, diagnostics, and versioning

Improved analysis with `/Qspectre` for providing mitigation assistance for Spectre Variant 1 (CVE-2017-5753). For more information, see [Spectre Mitigations in MSVC](https://devblogs.microsoft.com/cppblog/spectre-mitigations-in-msvc/).

## C++ Standard Library improvements

- [C++20 P0550R2 \(remove_cvref)](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2017/p0550r2.pdf) is complete.

- C++17 \<charconv> floating-point to_chars() has been improved: shortest chars_format::fixed is 60-80% faster, and shortest/precision chars_format::hex is complete.

- More algorithms have parallelized implementations: is_sorted(), is_sorted_until(), is_partitioned(), set_difference(), set_intersection(), is_heap(), is_heap_until().

- Improvements to `std::variant` to make it more optimizer-friendly, resulting in better generated code. Code inlining is now much better with `std::visit`.

- We've applied clang-format to the C++ Standard Library headers for improved readability.

- Improved throughput when several standard library features are compiled using `if constexpr`.

- Optimized the standard library physical design to avoid compiling parts of the standard library not #include'd, cutting in half the build time of an empty file that includes only \<vector>.

## Performance/throughput fixes

- Build throughput improvements, including the way the linker handles File I/O, and link time in PDB type merging and creation.

- Added basic support for OpenMP SIMD vectorization. You can enable it using the new CL switch `-openmp:experimental`. This option allows loops annotated with `#pragma omp simd` to potentially be vectorized. The vectorization isn't guaranteed, and loops annotated but not vectorized will get a warning reported. No SIMD clauses are supported, they're simply ignored with a warning reported.

- Added a new inlining command-line switch `-Ob3`, which is a more aggressive version of `-Ob2`. `-O2` (optimize the binary for speed) still implies `-Ob2` by default. If you find that the compiler doesn't inline aggressively enough, consider passing `-O2 -Ob3`.

- To support hand vectorization of loops with calls to math library functions, and certain other operations like integer division, we've added support for Short Vector Math Library (SVML) intrinsic functions. These functions compute the 128-bit, 256-bit, or 512-bit vector equivalents. See the [Intel Intrinsic Guide](https://software.intel.com/sites/landingpage/IntrinsicsGuide/#!=undefined&techs=SVML) for definitions of the supported functions.

- New and improved optimizations:

  - Constant-folding and arithmetic simplifications for expressions using SIMD vector intrinsics, for both float and integer forms.

  - A more powerful analysis for extracting information from control flow (if/else/switch statements) to remove branches always proven to be true or false.

  - Improved memset unrolling to use SSE2 vector instructions.

  - Improved removal of useless struct/class copies, especially for C++ programs that pass by value.

  - Improved optimization of code using `memmove`, such as `std::copy` or `std::vector` and `std::string` construction.

## C++ IDE

### Live Share C++ support

[Live Share](/visualstudio/liveshare/) now supports C++, allowing developers using Visual Studio or Visual Studio Code to collaborate in real time. For more information, see [Announcing Live Share for C++: Real-Time Sharing and Collaboration](https://devblogs.microsoft.com/cppblog/cppliveshare/)

### IntelliCode for C++

IntelliCode is an optional extension that uses its own extensive training and your code context to put what you’re most likely to use at the top of your completion list. It can often eliminate the need to scroll down through the list. For C++, IntelliCode offers the most help when using popular libraries such as the Standard Library. For more information, see [AI-Assisted Code Completion Suggestions Come to C++ via IntelliCode](https://devblogs.microsoft.com/cppblog/cppintellicode/).

### Template IntelliSense

The **Template Bar** now uses the **Peek Window** UI rather than a modal window, supports nested templates, and pre-populates any default arguments into the **Peek Window**. For more information, see [Template IntelliSense Improvements for Visual Studio 2019 Preview 2](https://devblogs.microsoft.com/cppblog/template-intellisense-improvements-for-visual-studio-2019-preview-2/). A **Most Recently Used** dropdown in the **Template Bar** enables you to quickly switch between previous sets of sample arguments.

### New Start window experience

When launching the IDE, a new Start window appears with options to open recent projects, clone code from source control, open local code as a solution or a folder, or create a new project. The New Project dialog has also been overhauled into a search-first, filterable experience.

### New names for some project templates

We've modified several project template names and descriptions to fit with the updated New Project dialog.

### Various productivity improvements

Visual Studio 2019 includes the following features that will help make coding easier and more intuitive:

- Quick fixes for:
  - Add missing #include
  - NULL to nullptr
  - Add missing semicolon
  - Resolve missing namespace or scope
  - Replace bad indirection operands (\* to & and & to \*)
- Quick Info for a block by hovering on closing brace
- Peek Header / Code File
- Go to Definition on #include opens the file

For more information, see [C++ Productivity Improvements in Visual Studio 2019 Preview 2](https://devblogs.microsoft.com/cppblog/c-productivity-improvements-in-visual-studio-2019-preview-2/).

## CMake support

- Visual Studio can now open existing CMake caches generated by external tools, such as CMakeGUI, customized meta-build systems or build scripts that invoke cmake.exe themselves.

- Improved IntelliSense performance.

- A new settings editor provides an alternative to manually editing the CMakeSettings.json file, and provides some parity with CMakeGUI.

- Visual Studio helps bootstrap your C++ development with CMake on Linux by detecting if you have a compatible version of CMake on your Linux machine, and if not offers to install it for you.

- Incompatible settings in CMakeSettings, such as mismatched architectures or incompatible CMake generator settings, show squiggles in the JSON editor and errors in the error list.

- The vcpkg toolchain is automatically detected and enabled for CMake projects that are opened in the IDE once `vcpkg integrate install` has been run. This behavior can be turned off by specifying an empty toolchain file in CMakeSettings.

- CMake projects now enable Just My Code debugging by default.

- Static analysis warnings can now be processed in the background and displayed in the editor for CMake projects.

- Clearer build and configure 'begin' and 'end' messages for CMake projects and support for Visual Studio's build progress UI. Additionally, there's now a CMake verbosity setting in **Tools > Options** to customize the detail level of CMake build and configuration messages in the Output Window.

- The `cmakeToolchain` setting is now supported in CMakeSettings.json to specify toolchains without manually modifying the CMake command line.

- A new **Build All** menu shortcut **Ctrl+Shift+B**.

## Debugging

- For C++ applications running on Windows, PDB files now load in a separate 64-bit process. This change addresses a range of crashes caused by the debugger running out of memory when debugging applications that contain a large number of modules and PDB files.

## Windows desktop development with C++

- These C++ ATL/MFC wizards are no longer available:

  - ATL COM+ 1.0 Component Wizard
  - ATL Active Server Pages Component Wizard
  - ATL OLE DB Provider Wizard
  - ATL Property Page Wizard
  - ATL OLE DB Consumer Wizard
  - MFC ODBC Consumer
  - MFC class from ActiveX control
  - MFC class from Type Lib.

  Sample code for these technologies is archived at Microsoft Docs and the VCSamples GitHub repository.

- The Windows 8.1 SDK is no longer available in the Visual Studio installer. We recommend you upgrade your C++ projects to the latest Windows 10 SDK. If you have a hard dependency on 8.1, you can download it from the Windows SDK archive.

- Windows XP targeting will no longer be available for the latest C++ toolset. XP targeting with VS 2017-level MSVC compiler & libraries is still supported and can be installed via "Individual components."

- Our documentation actively discourages usage of Merge Modules for Visual C++ Runtime deployment. We're taking the extra step this release of marking our MSMs as deprecated. Consider migrating your VCRuntime central deployment from MSMs to the redistributable package.

## Mobile development with C++ (Android and iOS)

The C++ Android experience now defaults to Android SDK 25 and Android NDK 16b.

## Clang/C2 platform toolset

The Clang/C2 experimental component has been removed. Use the MSVC toolset for full C++ standards conformance with `/permissive-` and `/std:c++17`, or the Clang/LLVM toolchain for Windows.

## Code analysis

- Code analysis now runs automatically in the background. Warnings display as green squiggles in-editor as you type. For more information, see [In-editor code analysis in Visual Studio 2019 Preview 2](https://devblogs.microsoft.com/cppblog/in-editor-code-analysis-in-visual-studio-2019-preview-2/).

- New experimental ConcurrencyCheck rules for well-known Standard Library types from the \<mutex> header. For more information, see [Concurrency Code Analysis in Visual Studio 2019](https://devblogs.microsoft.com/cppblog/concurrency-code-analysis-in-visual-studio-2019/).

- An updated partial implementation of the [Lifetime profile checker](https://herbsutter.com/2018/09/20/lifetime-profile-v1-0-posted/), which detects dangling pointers and references. For more information, see [Lifetime Profile Update in Visual Studio 2019 Preview 2](https://devblogs.microsoft.com/cppblog/lifetime-profile-update-in-visual-studio-2019-preview-2/).

- More coroutine-related checks, including C26138, C26810, C26811, and the experimental rule C26800. For more information, see [New Code Analysis Checks in Visual Studio 2019: use-after-move and coroutine](https://devblogs.microsoft.com/cppblog/new-code-analysis-checks-in-visual-studio-2019-use-after-move-and-coroutine/).

## Unit testing

The Managed C++ Test Project template is no longer available. You can continue using the Managed C++ Test framework in your existing projects. For new unit tests, consider using one of the native test frameworks for which Visual Studio provides templates (MSTest, Google Test), or the Managed C# Test Project template.
