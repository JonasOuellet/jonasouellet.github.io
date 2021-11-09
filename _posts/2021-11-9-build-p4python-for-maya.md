How to compile p4python for Autodesk Maya.

Autodesk Maya 2020 and lower uses a python27 version compiled with Visual Studio 2015 (v140 build tools).  This restrains the utilisation of p4python version installed directly with pip. To use p4python in Maya, it needs to be compiled using the same build tools.

You can download the already compiled version [here](https://1drv.ms/u/s!AmUIPYb_04PSge0RmEy9TFH1CdNHwA?e=OFFFbe)

## OpenSSL

The last version of p4python must be compiled with Openssl. To do so, we also need to compile it with visual studio 2015. 

## Prerequisites

- Download and install [visual studio 2015 build tools](https://www.microsoft.com/en-us/download/details.aspx?id=48159&WT.mc_id=rss_alldownloads_devresources)
- Install [Strawberry Perl](https://strawberryperl.com/).
- Install [NASM](https://www.nasm.us/)

## Sources

- Download openssl version 1.1.1x [source code](https://www.openssl.org/source/)
- Download p4api version 2021.1 [source code](http://ftp.perforce.com/perforce/r21.1/bin.ntx64/p4api_vs2015_dyn_openssl1.1.1.zip).  P4 version 2021.1 is the last version that support python 2.7.
- Download p4python [source code](http://ftp.perforce.com/perforce/r21.1/bin.tools/p4python.tgz)


## Compile OpenSSL

OpenSSL can be build statically or dynamicly.  If we build statically, it will be included into the resulting p4python library.  This means that users using p4python, won't need to have openssl library installed.  Since we want to having any external dependencies, we will build openssl statically with the `no-shared` keyword.

If you don't have nasm installed, you can specify `no-asm` to build without nasm.  Withou nasm, some function might be slower.

The first line, activate visual studio build tools.  Then, we configure openssl build for win x64  You must replace `{openssl out path}` with the desired output path.

Open an administrator command prompt and navigate to openssl folder.
```bash
"C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\bin\amd64\vcvars64.bat"
perl Configure VC-WIN64A no-shared --prefix={openssl out path}.
nmake
nmake install
```

## Compile P4Python

To step to build p4python are fairly simples.  Open an administrator command prompt and navigate to p4python folder.

With `INCLUDE` we can tell visual studio where are python headers files.

Replace `{version}` with your installed maya version.  The path might be different
if you've installed Maya to a custom location.

Replace `{openssl out path}` with the output path used above.  Path must be absolute.

Replace `{p4api folder}` with the p4api downloaded before.  Path must be absolute.

```bash
"C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\bin\amd64\vcvars64.bat"
set INCLUDE=C:\Program Files\Autodesk\Maya{version}\include\Python
C:\Program Files\Autodesk\Maya{version}\bin\mayapy.exe setup.py build --apidir {p4api folder} --ssl {openssl out path}\lib
```
