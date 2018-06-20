WARNING : LLVM-6.0.0 Port attempt of `obfuscator-llvm/obfuscator`

Compile :
  # Clone the repo
  git clone https://github.com/spelle/obfuscator obfuscator.src
  # Retrieve dependencies
  curl -fsSL http://releases.llvm.org/6.0.0/cfe-6.0.0.src.tar.xz -o cfe-6.0.0.src.tar.xz
  curl -fsSL http://releases.llvm.org/6.0.0/compiler-rt-6.0.0.src.tar.xz -o compiler-rt-6.0.0.src.tar.xz
  curl -fsSL http://releases.llvm.org/6.0.0/libcxx-6.0.0.src.tar.xz -o libcxx-6.0.0.src.tar.xz
  curl -fsSL http://releases.llvm.org/6.0.0/libcxxabi-6.0.0.src.tar.xz -o libcxxabi-6.0.0.src.tar.xz
  curl -fsSL http://releases.llvm.org/6.0.0/libunwind-6.0.0.src.tar.xz -o libunwind-6.0.0.src.tar.xz
  curl -fsSL http://releases.llvm.org/6.0.0/lld-6.0.0.src.tar.xz -o lld-6.0.0.src.tar.xz
  curl -fsSL http://releases.llvm.org/6.0.0/lldb-6.0.0.src.tar.xz -o lldb-6.0.0.src.tar.xz
  curl -fsSL http://releases.llvm.org/6.0.0/openmp-6.0.0.src.tar.xz -o openmp-6.0.0.src.tar.xz
  curl -fsSL http://releases.llvm.org/6.0.0/polly-6.0.0.src.tar.xz  -o polly-6.0.0.src.tar.xz
  curl -fsSL http://releases.llvm.org/6.0.0/clang-tools-extra-6.0.0.src.tar.xz -o clang-tools-extra-6.0.0.src.tar.xz
  curl -fsSL http://releases.llvm.org/6.0.0/test-suite-6.0.0.src.tar.xz -o test-suite-6.0.0.src.tar.xz
  for f in $(ls *.tar.xz) ; do tar xf $f ; done

  cd obfuscator.src
  # Create links for projects
  cd projects
  ln -snf ../../compiler-rt-6.0.0.src compiler-rt
  ln -snf ../../libcxx-6.0.0.src libcxx
  ln -snf ../../libcxxabi-6.0.0.src libcxxabi
  ln -snf ../../libunwind-6.0.0.src libunwind
  ln -snf ../../openmp-6.0.0.src openmp
  ln -snf ../../test-suite-6.0.0.src test-suite
  cd ..
  #Create links for tools
  cd tools
  ln -snf ../../cfe-6.0.0.src clang
  ln -snf ../../lld-6.0.0.src lld
  ln -snf ../../lldb-6.0.0.src lldb
  ln -snf ../../polly-6.0.0.src polly
  cd clang
  cd tools
  ln -snf ../../clang-tools-extra-6.0.0.src extra
  cd ..
  cd ..
  cd ..
  cd ..
  mkcd obfuscator.build
  cmake -G "Unix Makefiles" -DLLDB_CODESIGN_IDENTITY='' ../llvm
  make -j7



Please have a look at the [wiki](https://github.com/c/wiki)!

Current version: [LLVM-4.0](https://github.com/obfuscator-llvm/obfuscator/tree/llvm-4.0)

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
