# BRAMS 6.0 - INSTALL PREREQUISITES

Before You install the prerequisites You must have at least one version of gfortran and gcc compiler. The instalation depends of the version of Linux and Kernel You have.

> We recomend to install the **gfortran revision 8.4.0**  and **gcc revision 8.4.0**. You can install or use newest version of compilers but is by your own risk. You may try to use INTEL, NVIDIA or other compiler too. The strucuture of setup is almost the same.

> In body of text bellow we will use  **<Your_C_compiler>** for your C compiler choice and <Your_Fortran_compiler> for Your Fortran compiler choice.

In most system there are a lot of libraries pre-installed. Yes, if possible use them. But, unfortunatelly, some times a library require other dependencies and the configure may be broken. The instructions bellow in this documents will guide You to install in a particular way. If You got others problems please, contact your system manager.

1. ## Create the prerequisites folder's strucuture:
   
   You will need a folder (**{YOUR_DIR}**) to put the libraries, includes and executables. You can choose a system folder or a local home folder. If You prefer a system folder remember that commands must be preceeded by a sudo command. Another folder (**{INSTALL_DIR}**) will be used only for create the sctrucuture and can bem erased after all instalation.
   
   ```bash
   mkdir {YOUR_DIR}
   mkdir {YOUR_DIR}/bin
   mkdir {YOUR_DIR}/lib
   mkdir {YOUR_DIR}/include
   mkdir {INSTALL_DIR}
   cd {INSTALL_DIR}
   ```
   
   example (using system folder /opt/apps as {YOUR_DIR} and user home folder /home/oscar as {INSTALL_DIR}):
   
   ```bash
   sudo mkdir /opt/apps
   sudo mkdir /opt/apps/bin
   sudo mkdir /opt/apps/lib
   sudo mkdir /opt/apps/include
   mkdir /home/oscar/install
   cd /home/oscar/install
   ```
   
   >  If You will use differents compilers environment we recommend do create the folder with respective names like /opt/nvidia , /opt/intel or /opt/gnu instead /opt/apps as example above

2. ## Download all necessary prerequisites
   
   In order to build full prerequisites a bunch of packages must be downloaded. You can do it in just one command by getting the packages from FTP BRAMS' area or getting every package individually.  
   
   To get just one file (easyest way) with all libraries please, get it as show bellow
   
   ```bash
   wget http://ftp.cptec.inpe.br/pesquisa/bramsrd/BRAMS/pre-requisites/prerequisites.tar
   tar -xvf prerequisites.tar
   ```
   
   The necessary packages are:
   
   | Package        | Site                                                   | File                        |
   |:--------------:|:------------------------------------------------------:|:---------------------------:|
   | mpich3         | http://www.mpich.org/static/downloads/4.0a2/           | mpich-4.0a2.tar.gz          |
   | zlib           | ftp://ftp.unidata.ucar.edu/pub/netcdf/netcdf-4/        | zlib-1.2.8.tar.gz           |
   | szip           | ftp://ftp.unidata.ucar.edu/pub/netcdf/netcdf-4/        | szip-2.1.tar.gz             |
   | curl           | ftp://ftp.unidata.ucar.edu/pub/netcdf/netcdf-4/        | curl-7.26.0.tar.gz          |
   | netCDF C       | https://downloads.unidata.ucar.edu/netcdf-c/4.8.1/src/ | netcdf-c-4.8.1.tar.gz       |
   | netCDF Fortran | https://www.unidata.ucar.edu/downloads/netcdf/ftp/     | netcdf-fortran-4.5.3.tar.gz |
   | grib2          | https://www.ftp.cpc.ncep.noaa.gov/wd51we/wgrib2/       | wgrib2.tgz                  |
   | hdf5           | https://www.hdfgroup.org/downloads/hdf5/source-code/   | hdf5-1.12.1.tar.gz          |
   
   > Notice: the grib2 file has diferent names if You get it from NCEP or if You get it from BRAMS ftp.

3. You must have sure about where are the libraries, includes and binaries You will build the system.  Is necessary to make a symbolic link between the gfortran compiler (version 8) of your system using just "gfortran" in your bin area. The same for gcc. Let's setup the paths:
   
   ```batch
   export PATH={YOUR_DIR}/bin:$PATH
   export LD_LIBRARY_PATH={YOUR_DIR}/lib:$LD_LIBRARY_PATH
   sudo ln -s /usr/bin/gfortran {YOUR_DIR}/bin/gfortran
   sudo ln -s /usr/bin/gcc {YOUR_DIR}/bin/gcc
   ```
   
   > If you will use other compiler instead gfortran/gcc make the export for correct compiler
   
   Please, check if Your path have in first part {YOUR_DIR} and if the correct library is in first part of LD_LIBRARY_PATH. 
   
   ```batch
   echo $PATH
   echo $LD_LIBRARY_PATH
   ```
   
   Please check if Fortran and gcc version is the correct or make the same for the other compiler You will use.
   
   ```
   gfortran --version
   gcc --version
   ```
   
   The result must be something like. See that in this case we use 8.4.0.
   
   ```
   $ gfortran --version
   GNU Fortran (Ubuntu 8.4.0-4ubuntu1) 8.4.0
   Copyright (C) 2018 Free Software Foundation, Inc.
   This is free software; see the source for copying conditions.  There is NO
   warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
   $ gcc --version
   gcc (Ubuntu 8.4.0-4ubuntu1) 8.4.0
   Copyright (C) 2018 Free Software Foundation, Inc.
   This is free software; see the source for copying conditions.  There is NO
   warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
   ```

4. ## Building the mpich libraries and binaries
   
   Now You must build all libraries one by one. Let's start with mpich
   
   ```bash
   tar -xzvf mpich-4.0a2.tar.gz 
   cd mpich-4.0a2/
   ./configure -disable-fast CC=<Your_C_compiler> FC=<Your_fortran_compiler> CFLAGS=-O2 FFLAGS=-O2 CXXFLAGS=-O2 FCFLAGS=-O2 --prefix={YOUR_DIR} --with-device=ch3
   make
   sudo make install
   cd ..
   ```

5. ## Building zlib, szip and curl
   
   ```bash
   #zlib
   tar -xzvf zlib-1.2.8.tar.gz 
   cd zlib-1.2.8/
   CC=<Your_C_compiler> ./configure --prefix={YOUR_DIR}
   make
   sudo make install
   cd ..
   
   #szip
   tar -xzvf szip-2.1.tar.gz 
   cd szip-2.1/
   CC=<Your_C_compiler> ./configure --prefix={YOUR_DIR}
   make
   sudo make install
   cd ..
   
   #Curl
   tar -xzvf curl-7.26.0.tar.gz 
   cd curl-7.26.0/
   CC=<Your_C_compiler> ./configure --prefix={YOUR_DIR} --without-libssh2
   make
   make install
   cd ..
   ```

6. ## Building HDF5
   
   ```bash
   tar -xzvf hdf5-1.12.1.tar.gz 
   cd hdf5-1.12.1/
   ./configure --prefix={YOUR_DIR} CC={YOUR_DIR}/bin/mpicc FC={YOUR_DIR}/bin/mpif90 --with-zlib={YOUR_DIR} --with-szlib={YOUR_DIR} --enable-parallel --enable-fortran
   make
   sudo make install
   cd ..
   ```
   
    If You used **NVIDIA** (pgf90/pgcc) to build mpich, instead the configure above,  make the configure using the command lines bellow
   
   ```bash
   ./configure --prefix={YOUR_DIR} FFLAGS=-fPIC FCFLAGS=-fPIC CC={YOUR_DIR}/bin/mpicc FC={YOUR_DIR}/bin/mpif90 --with-zlib={YOUR_DIR} --with-szlib={YOUR_DIR} --enable-parallel --enable-fortran
   make
   sudo make install
   cd ..
   ```

7. ## Building NetCDF libraries
   
   ```bash
   tar -xzvf netcdf-c-4.8.1.tar.gz 
   cd netcdf-c-4.8.1/
   CPPFLAGS=-I{YOUR_DIR}/include LDFLAGS=-L{YOUR_DIR}/lib CFLAGS='-O3'  CC={YOUR_DIR}/bin/mpicc ./configure --prefix={YOUR_DIR} --enable-netcdf4 --enable-shared --enable-dap
   make
   make install
   cd ..
   
   tar -xzvf netcdf-fortran-4.5.3.tar.gz
   cd netcdf-fortran-4.5.3/
   CPPFLAGS=-I{YOUR_DIR}/include LDFLAGS=-L{YOUR_DIR}/lib CFLAGS='-O3' FC={YOUR_DIR}/bin/mpif90  CC={YOUR_DIR}/bin/mpicc ./configure --prefix={YOUR_DIR}
   make
   sudo make install
   cd ..
   ```
   
   If You are using NVIDIA (pgf90/pgcc), to build the NetCDF-fortran, instead the configure above, make the configure using the command lines bellow
   
   ```bash
   FFLAGS=-fPIC FCFLAGS=-fPIC CPPFLAGS=-I{YOUR_DIR}/include LDFLAGS=-L{YOUR_DIR}/lib CFLAGS='-O3' FC={YOUR_DIR}/bin/mpif90  CC={YOUR_DIR}/bin/mpicc ./configure --prefix={YOUR_DIR}
   make
   sudo make install
   cd ..
   ```

8. ## Building grib2 libraries and API

```bash
tar -xzvf grib2.tgz
cd grib2
```

   Before compile You must modify the makefile. <u>Edit the makefile</u>, find and change the following variables (use the values as show bellow):

```makefile
USE_NETCDF3=0
USE_NETCDF4=0
USE_REGEX=1
USE_TIGGE=1
USE_MYSQL=0
USE_IPOLATES=3
USE_SPECTRAL=0
USE_UDF=0
USE_OPENMP=0
USE_PROJ4=0
USE_WMO_VALIDATION=0
DISABLE_TIMEZONE=0
USE_NAMES=NCEP
MAKE_FTN_API=1
DISABLE_ALARM=0
MAKE_SHARED_LIB=0

USE_G2CLIB=0
USE_PNG=0
USE_JASPER=0
USE_OPENJPEG=0
USE_AEC=0
```

If You are using **NVIDIA compilers** (pgf90/pgcc) You must change other parts of makefile. Find the commands sequence:

```makefile
   system=$(shell uname -s)
   ifeq (${system},Linux)
      ifeq ($(findstring gcc,$(notdir $(CC))),gcc)
         COMP_SYS=gnu_linux
      endif
      ifeq ($(findstring g95,$(notdir $(FC))),g95)
         COMP_SYS=gnu_linux_g95
      endif
      ifeq ($(findstring clang,$(notdir $(CC))),clang)
         COMP_SYS=clang_linux
      endif
      ifeq ($(findstring icc,$(notdir $(CC))),icc)
         COMP_SYS=intel_linux
      endif
      ifeq ($(findstring nvc,$(notdir $(CC))),nvc)
         COMP_SYS=nvidia_linux
      endif
   endif
```

Now add the lines bellow at end of the sequence You found

```makefile
  ifeq ($(findstring pgcc,$(notdir $(CC))),pgcc)
     COMP_SYS=nvidia_linux
  endif
```

The result must be:

```makefile
   system=$(shell uname -s)
   ifeq (${system},Linux)
      ifeq ($(findstring gcc,$(notdir $(CC))),gcc)
         COMP_SYS=gnu_linux
      endif
      ifeq ($(findstring g95,$(notdir $(FC))),g95)
         COMP_SYS=gnu_linux_g95
      endif
      ifeq ($(findstring clang,$(notdir $(CC))),clang)
         COMP_SYS=clang_linux
      endif
      ifeq ($(findstring icc,$(notdir $(CC))),icc)
         COMP_SYS=intel_linux
      endif
      ifeq ($(findstring nvc,$(notdir $(CC))),nvc)
         COMP_SYS=nvidia_linux
      endif
      ifeq ($(findstring pgcc,$(notdir $(CC))),pgcc)
         COMP_SYS=nvidia_linux
        endif
   endif
```

Find the following sequence:

```makefile
ifeq ($(need_ftn),1)
  ifeq ($(findstring gfortran,$(notdir $(FC))),gfortran)
    a:=$(shell echo '$Hdefine FORTRAN '\"`${FC} --version | head -n 1`\" >> ${CONFIG_H})
  else ifeq ($(findstring ifort,$(notdir $(FC))),ifort)
    a:=$(shell echo '$Hdefine FORTRAN '\"`${FC} --version | head -n 1`\" >> ${CONFIG_H})
  else ifeq ($(findstring nvfortran,$(notdir $(FC))),nvfortran)
    a:=$(shell echo '$Hdefine FORTRAN '\"`${FC} --version | head -n 2 | tail -n 1`\" >> ${CONFIG_H})
  else
    a:=$(shell echo '$Hdefine FORTRAN '\"${FC}\" >> ${CONFIG_H})
  endif
endif
```

 add the sequence lines after nvfortran section:

```makefile
else ifeq ($(findstring pgf90,$(notdir $(FC))),pgf90)
   a:=$(shell echo '$Hdefine FORTRAN '\"`${FC} --version | head -n 2 | tail -n 1`\" >> ${CONFIG_H})
```

The results must be

```makefile
ifeq ($(need_ftn),1)
  ifeq ($(findstring gfortran,$(notdir $(FC))),gfortran)
    a:=$(shell echo '$Hdefine FORTRAN '\"`${FC} --version | head -n 1`\" >> ${CONFIG_H})
  else ifeq ($(findstring ifort,$(notdir $(FC))),ifort)
    a:=$(shell echo '$Hdefine FORTRAN '\"`${FC} --version | head -n 1`\" >> ${CONFIG_H})
  else ifeq ($(findstring nvfortran,$(notdir $(FC))),nvfortran)
    a:=$(shell echo '$Hdefine FORTRAN '\"`${FC} --version | head -n 2 | tail -n 1`\" >> ${CONFIG_H})
  else ifeq ($(findstring pgf90,$(notdir $(FC))),pgf90)
    a:=$(shell echo '$Hdefine FORTRAN '\"`${FC} --version | head -n 2 | tail -n 1`\" >> ${CONFIG_H})
  else
    a:=$(shell echo '$Hdefine FORTRAN '\"${FC}\" >> ${CONFIG_H})
  endif
endif
```

With the modifications You can now compile and install:

```bash
make CC=<Your_C_compiler> FC=<Your_Fortran_compiler>
make CC=<Your_C_compiler> FC=<Your_Fortran_compiler> lib
sudo cp wgrib2/wgrib2 {YOUR_DIR}/bin/
sudo cp wgrib2/libwgrib2.a {YOUR_DIR}/lib/
sudo cp ./lib/*.a {YOUR_DIR}/lib/
sudo cp ./lib/*.mod {YOUR_DIR}/include/
cd ..
```

9. ## Creating alias for use

If You compile for a specific compiler, gnu, nvidia or intel (or others) all the libraries and binaries will be build in the compiler. For correct use keep in mind that the PATHs must be pointed to correct binary. For example: You will need the mpirun binary for run the model. We recommend You create some alias to call before the use of each run model. Edit the .bashrc in Your home area and put the folowwing lines:

```bash
alias gnu8='export PATH=/opt/gnu8/bin:$PATH;export LD_LIBRARY_PATH=/opt/gnu8/lib:$LD_LIBRARY_PATH'
alias nvidia='export PATH=/opt/nvidia/bin:$PATH;export LD_LIBRARY_PATH=/opt/nvidia/lib:$LD_LIBRARY_PATH'
```

In the example we create two alias, the first "gnu8" adjust the path to search in /opt/gnu8/bin first and use the libs from /opt/gnu8/lib. At same way the nvidia alias but change the pointer to nvidia area.

After edit and save just type on terminal 

```bash
source ~/.bashrc 
```

and the two alias will be available. Now You can choose the correct before run the model. If You will run the gnu install execute the alias gnu8 on terminal and You will be ready to go.

---



```
   All the required prerequisites are installed. Have fun!
```
