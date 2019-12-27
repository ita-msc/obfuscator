Please have a look at the [wiki](https://github.com/obfuscator-llvm/obfuscator/wiki)!

Current (official) version: [LLVM-4.0](https://github.com/obfuscator-llvm/obfuscator/tree/llvm-4.0)

Current version on this repo is [LLVM-9.0.1](https://github.com/ita-msc/obfuscator/tree/llvm-9.0.1)

You can cite Obfuscator-LLVM using the following Bibtex entry:

```
@INPROCEEDINGS{ieeespro2015-JunodRWM,
  author={Pascal Junod and Julien Rinaldini and Johan Wehrli and Julie Michielin},
  booktitle={Proceedings of the {IEEE/ACM} 1st International Workshop on Software Protection, {SPRO'15}, Firenze, Italy, May 19th, 2015},
  editor = {Brecht Wyseur},
  publisher = {IEEE},
  title={Obfuscator-{LLVM} -- Software Protection for the Masses},
  year={2015},
  pages={3--9},
  doi={10.1109/SPRO.2015.10},
}
```

# How to Build :

  ## Clone the repo

```
git clone https://github.com/ita-msc/obfuscator -b llvm-9.0.1 obfuscator-llvm-9.0.1.src
```

  ## Retrieve dependencies

```
curl -E -fsSL http://releases.llvm.org/9.0.1/cfe-9.0.1.src.tar.xz -o cfe-9.0.1.src.tar.xz && \
curl -E -fsSL http://releases.llvm.org/9.0.1/compiler-rt-9.0.1.src.tar.xz -o compiler-rt-9.0.1.src.tar.xz && \
curl -E -fsSL http://releases.llvm.org/9.0.1/libcxx-9.0.1.src.tar.xz -o libcxx-9.0.1.src.tar.xz && \
curl -E -fsSL http://releases.llvm.org/9.0.1/libcxxabi-9.0.1.src.tar.xz -o libcxxabi-9.0.1.src.tar.xz && \
curl -E -fsSL http://releases.llvm.org/9.0.1/libunwind-9.0.1.src.tar.xz -o libunwind-9.0.1.src.tar.xz && \
curl -E -fsSL http://releases.llvm.org/9.0.1/lld-9.0.1.src.tar.xz -o lld-9.0.1.src.tar.xz && \
curl -E -fsSL http://releases.llvm.org/9.0.1/lldb-9.0.1.src.tar.xz -o lldb-9.0.1.src.tar.xz && \
curl -E -fsSL http://releases.llvm.org/9.0.1/openmp-9.0.1.src.tar.xz -o openmp-9.0.1.src.tar.xz && \
curl -E -fsSL http://releases.llvm.org/9.0.1/polly-9.0.1.src.tar.xz  -o polly-9.0.1.src.tar.xz && \
curl -E -fsSL http://releases.llvm.org/9.0.1/clang-tools-extra-9.0.1.src.tar.xz -o clang-tools-extra-9.0.1.src.tar.xz && \
curl -E -fsSL http://releases.llvm.org/9.0.1/test-suite-9.0.1.src.tar.xz -o test-suite-9.0.1.src.tar.xz && \
for f in $(ls *.tar.xz) ; do tar xf $f ; done
```

  ## Create links for projects

```
cd obfuscator-llvm-9.0.1.src && \
cd projects && \
ln -snf ../../compiler-rt-9.0.1.src compiler-rt && \
ln -snf ../../libcxx-9.0.1.src libcxx && \
ln -snf ../../libcxxabi-9.0.1.src libcxxabi && \
ln -snf ../../libunwind-9.0.1.src libunwind && \
ln -snf ../../openmp-9.0.1.src openmp && \
ln -snf ../../test-suite-9.0.1.src test-suite && \
cd ..
```

  ## Create links for tools

```
cd tools && \
ln -snf ../../cfe-9.0.1.src clang && \
ln -snf ../../lld-9.0.1.src lld && \
ln -snf ../../lldb-9.0.1.src lldb && \
ln -snf ../../polly-9.0.1.src polly && \
cd clang && \
cd tools && \
ln -snf ../../clang-tools-extra-9.0.1.src extra && \
cd .. && \
cd .. && \
cd .. && \
cd ..
```

 ## Launch the build

Using `cmake` & `make`

```
mkdir obfuscator-llvm-9.0.1.build
cd obfuscator-llvm-9.0.1.build
cmake -G "Unix Makefiles" -DLLDB_CODESIGN_IDENTITY='' ../obfuscator-llvm-9.0.1.src
make -j7
```

Following flags can be added to the `cmake` command line

```
-DLLVM_ENABLE_ASSERTIONS=ON 
-DCMAKE_BUILD_TYPE=Release
```

### Special case for macOS & Xcode

From : https://afnan.io/2018-10-01/using-the-latest-llvm-release-on-macos/

```
cmake -G Ninja \
  -DLLDB_CODESIGN_IDENTITY='' \
  -DCMAKE_BUILD_TYPE=RelWithDebInfo \
  -DLLVM_CREATE_XCODE_TOOLCHAIN=On \
  -DLLVM_ENABLE_ASSERTIONS=ON \
  -DCMAKE_BUILD_TYPE=Release \
  ../obfuscator-llvm-9.0.1.src
ninja -j5
```

I also recommend using `Ninja` rather than  `make` to build LLVM, because it will build significantly faster.

Now that you have the XCode toolchain, you can place it in the Toolchains directory in XCode.

```
sudo ninja install-xcode-toolchain
```

The toolchain is generated and installed in `/usr/local/Toolchains/LLVM9.0.1.xctoolchain`

You need to instruct XCode to actually use the toolchain. You can do so in two ways: from your environment variables, and through the XCode app itself.

To set it through an environment variable:

```
export TOOLCHAINS="LLVM9.0.1"
```

In Xcode.app, you can select `Xcode -> Toolchains -> org.llvm.9.0.1` in the menu.

### Brew

Command line used :

```
cmake -G Unix Makefiles .. -DCMAKE_C_FLAGS_RELEASE=-DNDEBUG -DCMAKE_CXX_FLAGS_RELEASE=-DNDEBUG -DCMAKE_INSTALL_PREFIX=/usr/local/Cellar/llvm/9.0.0 -DCMAKE_BUILD_TYPE=Release
```

