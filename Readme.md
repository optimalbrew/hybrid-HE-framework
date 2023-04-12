# Framework for Hybrid Homomorphic Encryption

This repository contains code to test and benchmark symmetric ciphers in hybrid homomorphic encryption. This code is also used in the evaluations of the ciphers in the paper [1].

## HE libraries

The following Homomorphic Encryption Libraries are part of the framework:

- [HElib](https://github.com/homenc/HElib/) (version 2.2.2)
- [SEAL](https://github.com/Microsoft/SEAL/) (version 3.7.3)
- [TFHE](https://github.com/tfhe/tfhe) (version 1.1)

The libraries are included as [submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules) in this repository and will be downloaded to the `thirdparty` directory. Cloning this repo will setup the `.gitmodules` file and target directory structure, but *will not fetch* the contents. To do that run

```
git submodule init
```

followed by

```
git submodule update
```

## Ciphers

The following ciphers are already implemented in the framework:

- [LowMC](https://eprint.iacr.org/2016/687.pdf)
- [Rasta](https://eprint.iacr.org/2018/181.pdf)
- [Agrasta](https://eprint.iacr.org/2018/181.pdf)
- [Dasta](https://tosc.iacr.org/index.php/ToSC/article/view/8696/8288)
- [Kreyvium](https://eprint.iacr.org/2015/113.pdf)
- [FiLIP](https://eprint.iacr.org/2019/483.pdf)
- [Masta](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=9240936)
- [Hera](https://eprint.iacr.org/2020/1335.pdf)
- [Pasta](https://eprint.iacr.org/2021/731.pdf)

## Compilation

The framework is developed and tested on Linux (and WSL2). The dependencies of the framework are `git`, `gcc/g++` (or `clang/clang++`), `cmake`, `autoconf`, and `libtool`. Install these dependencies with

```bash
sudo apt install build-essential cmake autoconf libtool
```

To compile the framework, execute the following commands from the root directory:

```bash
mkdir build
cd build
cmake ..
make -j4
```

Each cipher for each library will then have its own executable, located in `build/test`.


### Errors while building

Note (by optimalbrew): The build process had errors. It was to not being able to `gmp.h`. So I [isntalled GMP](https://gmplib.org/manual/Installing-GMP).
The library fles were added installed at `usr/local/lib`.

Presumably an alternative (not tested) is to use `apt install libgmp3-dev`. 

---


## Testing

Similar to build. Move into the `tests` directory. To limit the time taken, comment out tests that aren't needed from `CMakeLists.txt` (in the main directory).


``` bash
cmake..
make
make test

```

The tests take a while and a bunch of selected ones failed 
```
Start  1: he_only_plain
 1/11 Test  #1: he_only_plain ....................   Passed    0.03 sec
      Start  2: he_only_z2_plain
 2/11 Test  #2: he_only_z2_plain .................   Passed    0.00 sec
      Start  3: pasta_3_plain
 3/11 Test  #3: pasta_3_plain ....................   Passed    0.13 sec
      Start  4: hera_5_plain
 4/11 Test  #4: hera_5_plain .....................   Passed    0.03 sec
      Start  5: he_only_seal
 5/11 Test  #5: he_only_seal .....................   Passed   64.99 sec
      Start  6: pasta_3_seal
6/11 Test  #6: pasta_3_seal .....................Subprocess killed***Exception: 476.57 sec
      Start  7: hera_5_seal
 7/11 Test  #7: hera_5_seal ......................Subprocess killed***Exception:  54.43 sec
      Start  8: he_only_helib
 8/11 Test  #8: he_only_helib ....................Subprocess killed***Exception:  58.95 sec
      Start  9: pasta_3_helib
 9/11 Test  #9: pasta_3_helib ....................Subprocess killed***Exception:  35.67 sec
      Start 10: hera_5_helib
10/11 Test #10: hera_5_helib .....................Subprocess killed***Exception:  74.77 sec
      Start 11: he_only_z2_tfhe
11/11 Test #11: he_only_z2_tfhe ..................   Passed  165.80 sec

55% tests passed, 5 tests failed out of 11

Total Test time (real) = 931.44 sec

```

see the log [failedTest.log](./failedTest.log) - which is not very informative, need to check details.



## Adding a cipher

A new cipher can be added to the framework by adding the source code to the `ciphers` directory. Make sure to use the same structure and naming convention as the already implemented ciphers! Furthermore, add the cipher to `CMakeLists.txt`.

## Benchmarking a cipher

The framework already includes different benchmarks, which are implemented in `ciphers/common/<lib>_kats` for Z_2 ciphers and in `ciphers/common_Zp/<lib>_kats` for Z_p ciphers. The benchmarks can be chosen by adding corresponding tests to the `testvectors.h` file of the cipher.

## Citing our work

Please use the following BibTeX entry to cite our work in academic papers.

```tex
@article{HybridHE,
  author    = {Christoph Dobraunig and
               Lorenzo Grassi and
               Lukas Helminger and
               Christian Rechberger and
               Markus Schofnegger and
               Roman Walch},
  title     = {Pasta: A Case for Hybrid Homomorphic Encryption},
  journal   = {{IACR} Cryptol. ePrint Arch.},
  volume    = {2021},
  pages     = {731},
  year      = {2021}
}
```

[1] [https://eprint.iacr.org/2021/731.pdf](https://eprint.iacr.org/2021/731.pdf)
