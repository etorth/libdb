### Version
Oracle Berkeley DB 12c Release 1 (Version 12.1.6.2.32)

### Who Uses It
- [mir2x](https://github.com/etorth/mir2x)

### Why It Is Needed
`mir2x` features a built-in Pinyin input method, providing out-of-the-box convenience for systems lacking a native Chinese input method. This feature is powered by `libpinyin`, which depends on Berkeley DB. While `libpinyin` can alternatively use Kyoto Cabinet, that alternative also requires patching. When using `vcpkg` as the project's build system, we encounter two main limitations:

- **Platform Restrictions:** The official `vcpkg` port for Berkeley DB only supports Windows (MSVC), whereas `mir2x` targets both Linux (Native/WSL2) and MinGW UCRT64.
- **Patching Requirements:** Using the upstream [Berkeley DB](https://github.com/berkeleydb/libdb.git) as an overlay port requires custom patching on MinGW.

Although the MinGW UCRT64 environment provides pre-compiled static and dynamic libraries for `libdb`, capturing those dependencies properly via `vcpkg` requires building from source. To streamline the `vcpkg` build pipeline for `mir2x`, this repository hosts a pre-patched version of the Berkeley DB source code.

### How This Patched Source Was Created
The source code in this repository was extracted directly from the MSYS2 upstream package recipes using the following steps:

- Open a **MinGW UCRT64** terminal.
- Clone the MSYS2 packages repository:
```bash
git clone https://github.com/msys2/MINGW-packages.git
```
- Navigate to the Berkeley DB package directory:
```bash
cd MINGW-packages/mingw-w64-db
```
- Download and patch the source code without running the full compilation:
```bash
makepkg-mingw -o
```
Upon completion, the contents of the `MINGW-packages/src/libdb` directory (which contains the fully patched source) were copied into the root of this repository. For tracking and reference, the original build recipes from `MINGW-packages/mingw-w64-db` were also copied into the `orig_mingw` directory.

### Original README
Berkeley DB 12c Release 1, library version 12.1.6.2.32: (April  5, 2017)

This is Berkeley DB 12c Release 1 from Oracle.  To view release and
installation documentation, load the distribution file docs/index.html
into your web browser.  Technical support can be obtained by visiting 
the Berkeley DB Oracle Technical Network (OTN) forum, here:

https://forums.oracle.com/forums/forum.jspa?forumID=271

