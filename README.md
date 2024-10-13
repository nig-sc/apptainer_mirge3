


## miRge3.0

The installation method is as follows.

``` bash
git clone https://github.com/nig-sc/apptainer_mirge3
cd apptainer_mirge3/mirge3
docker build -t mirge3 .
apptainer build mirge3.sif docker-daemon://mirge3:latest
```

The execution method is as follows. (TODO: Add more practical examples)

``` bash
$ apptainer exec mirge3.sif miRge3.0 --version
3.0
```

Additionally, the program listed in the official documentation under Installation has been installed in the container.

``` bash
$ apptainer shell mirge3.sif 
Apptainer> ls /usr/local/bin/
Kinfold      RNALfold	   RNAalifold	RNAdos	   RNAfold	RNAinverse  RNAparconv	RNAplfold  RNAsnoop   b2ct	   popt
RNA2Dfold    RNAPKplex	   RNAcofold	RNAduplex  RNAforester	RNAlocmin   RNApdist	RNAplot    RNAsubopt  ccache-swig  swig
RNALalifold  RNAaliduplex  RNAdistance	RNAeval    RNAheat	RNApaln     RNAplex	RNApvmin   RNAup      ct2db
Apptainer> exit
exit

```

The methods for invoking them are as follows.

```
$ apptainer exec mirge3.sif /usr/local/bin/Kinfold --help
Usage: Kinfold [OPTION]...
Stochastic Folding Simulations of Single-Stranded Nucleic Acids

  -h, --help            Print help and exit
      --full-help       Print help, including hidden options, and exit
  -V, --version         Print version and exit

Energy Model:
  -d, --dangle=INT      <0|1|2> set dangling end model to (none|normal|double)
                          (possible values="0", "1", "2" default=`2')
  -T, --Temp=FLOAT      simulation temperature  (default=`37')
  -P, --Par=filename    read energy-parameter-file
      --logML           use logarithmic multiloop energies instead of linear
                          (default=on)

(The rest is omitted.)
```



## mirge-build

The installation method is as follows.

``` bash
git clone https://github.com/nig-sc/apptainer_mirge3
cd apptainer_mirge3/mirge-build
docker build -t mirge-build .
apptainer build mirge-build.sif docker-daemon://mirge-build:latest
```

The execution method is as follows. (TODO: Add more practical examples)

``` bash
$ apptainer exec mirge-build.sif mirge-build --version 
3.0
```





