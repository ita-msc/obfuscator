

Please have a look at the [wiki](https://github.com/obfuscator-llvm/obfuscator/wiki)!

/!\ WARNING : This Branch / Project is attempt to port `obfuscator-llvm/obfuscator` to LLVM-7.0.0
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
git clone https://github.com/spelle/obfuscator -b llvm-7.0.1 obfuscator-llvm-7.0.1.src
```

  ## Retrieve dependencies

```
curl -E -fsSL http://releases.llvm.org/7.0.1/cfe-7.0.1.src.tar.xz -o cfe-7.0.1.src.tar.xz && \
curl -E -fsSL http://releases.llvm.org/7.0.1/compiler-rt-7.0.1.src.tar.xz -o compiler-rt-7.0.1.src.tar.xz && \
curl -E -fsSL http://releases.llvm.org/7.0.1/libcxx-7.0.1.src.tar.xz -o libcxx-7.0.1.src.tar.xz && \
curl -E -fsSL http://releases.llvm.org/7.0.1/libcxxabi-7.0.1.src.tar.xz -o libcxxabi-7.0.1.src.tar.xz && \
curl -E -fsSL http://releases.llvm.org/7.0.1/libunwind-7.0.1.src.tar.xz -o libunwind-7.0.1.src.tar.xz && \
curl -E -fsSL http://releases.llvm.org/7.0.1/lld-7.0.1.src.tar.xz -o lld-7.0.1.src.tar.xz && \
curl -E -fsSL http://releases.llvm.org/7.0.1/lldb-7.0.1.src.tar.xz -o lldb-7.0.1.src.tar.xz && \
curl -E -fsSL http://releases.llvm.org/7.0.1/openmp-7.0.1.src.tar.xz -o openmp-7.0.1.src.tar.xz && \
curl -E -fsSL http://releases.llvm.org/7.0.1/polly-7.0.1.src.tar.xz  -o polly-7.0.1.src.tar.xz && \
curl -E -fsSL http://releases.llvm.org/7.0.1/clang-tools-extra-7.0.1.src.tar.xz -o clang-tools-extra-7.0.1.src.tar.xz && \
curl -E -fsSL http://releases.llvm.org/7.0.1/test-suite-7.0.1.src.tar.xz -o test-suite-7.0.1.src.tar.xz && \
for f in $(ls *.tar.xz) ; do tar xf $f ; done
```

  ## Create links for projects

```
cd obfuscator-llvm-7.0.1.src && \
cd projects && \
ln -snf ../../compiler-rt-7.0.1.src compiler-rt && \
ln -snf ../../libcxx-7.0.1.src libcxx && \
ln -snf ../../libcxxabi-7.0.1.src libcxxabi && \
ln -snf ../../libunwind-7.0.1.src libunwind && \
ln -snf ../../openmp-7.0.1.src openmp && \
ln -snf ../../test-suite-7.0.1.src test-suite && \
cd ..
```

  ## Create links for tools

```
cd tools && \
ln -snf ../../cfe-7.0.1.src clang && \
ln -snf ../../lld-7.0.1.src lld && \
ln -snf ../../lldb-7.0.1.src lldb && \
ln -snf ../../polly-7.0.1.src polly && \
cd clang && \
cd tools && \
ln -snf ../../clang-tools-extra-7.0.1.src extra && \
cd .. && \
cd .. && \
cd .. && \
cd ..
```

 ## Launch the build

Using `cmake` & `make`

```
sudo mkdir obfuscator-llvm-7.0.1.build
sudo chown sds:sds obfuscator-llvm-7.0.1.build
cd obfuscator-llvm-7.0.1.build
cmake -G "Unix Makefiles" -DLLDB_CODESIGN_IDENTITY='' ../obfuscator-llvm-7.0.1.src
make -j7
```

Following flags can be added to the `cmake` command line

```
-DLLVM_ENABLE_ASSERTIONS=ON 
-DCMAKE_BUILD_TYPE=Release
```
