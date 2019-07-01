

Please have a look at the [wiki](https://github.com/obfuscator-llvm/obfuscator/wiki)!

/!\ WARNING : This Branch / Project is attempt to port `obfuscator-llvm/obfuscator` to LLVM-7.1.0
Current (official) version: [LLVM-4.0](https://github.com/obfuscator-llvm/obfuscator/tree/llvm-4.0)

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
git clone https://github.com/spelle/obfuscator -b llvm-7.1.0 obfuscator-llvm-7.1.0.src
```

  ## Retrieve dependencies

```
curl -E -fsSL http://releases.llvm.org/7.1.0/cfe-7.1.0.src.tar.xz -o cfe-7.1.0.src.tar.xz && \
curl -E -fsSL http://releases.llvm.org/7.1.0/compiler-rt-7.1.0.src.tar.xz -o compiler-rt-7.1.0.src.tar.xz && \
curl -E -fsSL http://releases.llvm.org/7.1.0/libcxx-7.1.0.src.tar.xz -o libcxx-7.1.0.src.tar.xz && \
curl -E -fsSL http://releases.llvm.org/7.1.0/libcxxabi-7.1.0.src.tar.xz -o libcxxabi-7.1.0.src.tar.xz && \
curl -E -fsSL http://releases.llvm.org/7.1.0/libunwind-7.1.0.src.tar.xz -o libunwind-7.1.0.src.tar.xz && \
curl -E -fsSL http://releases.llvm.org/7.1.0/lld-7.1.0.src.tar.xz -o lld-7.1.0.src.tar.xz && \
curl -E -fsSL http://releases.llvm.org/7.1.0/lldb-7.1.0.src.tar.xz -o lldb-7.1.0.src.tar.xz && \
curl -E -fsSL http://releases.llvm.org/7.1.0/openmp-7.1.0.src.tar.xz -o openmp-7.1.0.src.tar.xz && \
curl -E -fsSL http://releases.llvm.org/7.1.0/polly-7.1.0.src.tar.xz  -o polly-7.1.0.src.tar.xz && \
curl -E -fsSL http://releases.llvm.org/7.1.0/clang-tools-extra-7.1.0.src.tar.xz -o clang-tools-extra-7.1.0.src.tar.xz && \
curl -E -fsSL http://releases.llvm.org/7.1.0/test-suite-7.1.0.src.tar.xz -o test-suite-7.1.0.src.tar.xz && \
for f in $(ls *.tar.xz) ; do tar xf $f ; done
```

  ## Create links for projects

```
cd obfuscator-llvm-7.1.0.src && \
cd projects && \
ln -snf ../../compiler-rt-7.1.0.src compiler-rt && \
ln -snf ../../libcxx-7.1.0.src libcxx && \
ln -snf ../../libcxxabi-7.1.0.src libcxxabi && \
ln -snf ../../libunwind-7.1.0.src libunwind && \
ln -snf ../../openmp-7.1.0.src openmp && \
ln -snf ../../test-suite-7.1.0.src test-suite && \
cd ..
```

  ## Create links for tools

```
cd tools && \
ln -snf ../../cfe-7.1.0.src clang && \
ln -snf ../../lld-7.1.0.src lld && \
ln -snf ../../lldb-7.1.0.src lldb && \
ln -snf ../../polly-7.1.0.src polly && \
cd clang && \
cd tools && \
ln -snf ../../clang-tools-extra-7.1.0.src extra && \
cd .. && \
cd .. && \
cd .. && \
cd ..
```

 ## Launch the build

Using `cmake` & `make`

```
sudo mkdir obfuscator-llvm-7.1.0.build
cd obfuscator-llvm-7.1.0.build
cmake -G "Unix Makefiles" -DLLDB_CODESIGN_IDENTITY='' ../obfuscator-llvm-7.1.0.src
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
  ../obfuscator-llvm-7.1.0.src
ninja -j5
```

I also recommend using `Ninja` rather than  `make` to build LLVM, because it will build significantly faster.

Now that you have the XCode toolchain, you can place it in the Toolchains directory in XCode.

```
sudo ninja install-xcode-toolchain
```

The toolchain is generated and installed in `/usr/local/Toolchains/LLVM7.1.0.xctoolchain`

You need to instruct XCode to actually use the toolchain. You can do so in two ways: from your environment variables, and through the XCode app itself.

To set it through an environment variable:

```
export TOOLCHAINS="LLVM7.1.0"
```

In Xcode.app, you can select `Xcode -> Toolchains -> org.llvm.7.1.0` in the menu.


