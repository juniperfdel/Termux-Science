# Termux Science
How to install various python science packages: numpy, pandas, astropy, and jupyter notebook in termux on android (From the F-Droid store [here](https://f-droid.org/en/packages/com.termux/)) as of January 2023.

## Motivation
Most of the information I have seen on the internet on how to get these packages inside of termux on android is out of date as both termux and these packages have changed quite a bit over the last decade. While [Pydroid 3](https://play.google.com/store/apps/details?id=ru.iiec.pydroid3&hl=en_US&gl=US&pli=1) does provide python on android; I perfer being able to use python with other termux apps and scripts. 

## Disclaimer
This is just what worked for me and comes from reading Pull Request comments, this might have extra packages or flags.

## Commands
These should be run inside a termux controlled directory

```sh
pkg update -y
pkg install -y build-essential pkg-config freetype libpng libzmq zlib libxml2 fftw libandroid-execinfo libarrow-cpp libjpeg-turbo clang cmake ninja rust git python python-pip python-numpy python-static matplotlib libarrow-python
export CFLAGS="-Wno-deprecated-declarations -Wno-unreachable-code -Wno-int-conversion"
export MATHLIB="m"
export LDFLAGS="-lpython3.11"
export LD_PRELOAD="$LD_PRELOAD:/data/data/com.termux/files/usr/lib/libpython3.11.so.1.0"
pip install -vv pandas # -vv is because watching the loading thing for 40 minutes is not fun
pip install -vv astropy
pip install -vv notebook
```

## Scipy
Scipy is a very complex beast and currently the only thing that I can see as **the** blocker is a lack of a Fortran compiler (LAPACK is no longer fortran dependant, thank you [@martin-frbg](https://github.com/martin-frbg) for your [work](https://github.com/xianyi/OpenBLAS/pull/3539) and [this version of OpenBLAS got added](https://github.com/termux/termux-packages/pull/11441) ). The current [flang](https://github.com/llvm/llvm-project/tree/main/flang/) package in the main termux repos is broken and `scipy` doesn't recognize [lfortran](https://github.com/lfortran/lfortran) as a possible fortran compiler. [This comment](https://github.com/termux/termux-packages/issues/3719#issuecomment-1110005862) goes into compiling flang from source inside termux, and [this repo](https://github.com/buffer51/android-gfortran) seems to have done the work of getting the gnu toolchain inside of android (at least at one point), which includes gfortran, but I haven't tried either on my personal android device. Based on the [Scipy FAQ](https://scipy.org/faq/) it doesn't seem there is an appetite to move away from fortran, as it is a very integral part of it and the underlying libraries, so for the time being I don't see a relatively easy way to get scipy on android.

# Update as of Feburary 2024
The information here regarding scipy is out-of-date; as both [flang and scipy are now inside termux](https://github.com/termux/termux-packages/pull/18385)