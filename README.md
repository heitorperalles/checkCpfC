# checkCpfC

Command line tool to verify physical person registers status on Brazilian entities.<br>
A pure C solution that links with the library [CURL](https://curl.haxx.se) to manage HTTP requests.

The binary receives a CPF (Brazilian Physical Person Register) to be checked.<br>
Then the CPF is validated on SERPRO (Brazilian Federal Data Processing Service) that uses a RESTful API.<br>

The HTTP request implementation uses libCurl implementation ([github.com/curl](https://github.com/curl/curl)).

## Quick Start (on Linux)

### Install GNU gcc compiler
```bash
sudo apt install gcc
```
This application was tested with gcc release `9.3.0`. To check the installed version of gcc, run:
```bash
gcc --version
```

### Install lib Curl for Development
```bash
sudo apt-get install libcurl-dev
```
This application was tested with the library `libcurl4-openssl-dev` on release `7.68.0`

### Build and run checkCpfC (from the project root folder)
```bash
gcc -o checkCpfC *.c -lcurl
./checkCpfC 123456789
```
Assuming that *123456789* is a CPF to be checked.<br>
Attention: The `-lcurl` flag is necessary to link with the installed Curl library.

## Usage

Simply call the binary passing a CPF as parameter, the verdict will be printed on the command line.

### Success example

```
CPF:      40442820135
SUCCESS:  True
MESSAGE:  Regular CPF (OK)

```

### Error example

```
CPF:      11334739706
SUCCESS:  False
MESSAGE:  CPF not regular or not existant

```

## Testing the Binary

### Testing manually

#### Call the binary passing a test CPF as parameter:

```bash
./checkCpfC 40442820135
```

#### List of testing CPFs provided by SERPRO:

The application uses a service provided by SERPRO, so any test depends on this service being online.<br>
The CPFs provided by SERPRO for testing are:

```
40442820135: Regular (Register OK)
63017285995: Regular (Register OK)
91708635203: Regular (Register OK)
58136053391: Regular (Register OK)
40532176871: Suspended (Problem with the registry)
47123586964: Suspended (Problem with the registry)
07691852312: Regularization Pending (Problem with the registry)
10975384600: Regularization Pending (Problem with the registry)
01648527949: Canceled by Multiplicity (Problem with the registry)
47893062592: Canceled by Multiplicity (Problem with the registry)
98302514705: Null (Problem with the registry)
18025346790: Null (Problem with the registry)
64913872591: Registration Canceled (Problem with the registry)
52389071686: Registration Canceled (Problem with the registry)
05137518743: Deceased Holder (Problem with the registry)
08849979878: Deceased Holder (Problem with the registry)
```

## Implementation

### File structure

The application is composed by a set of source code files:

**app.c** : Main file, with `main()` function.<br>

**middleware.h** : C header file with definitions of functions and SERPRO structures.<br>
**middleware.c** : C source file with the core implementation of the REQUEST to SERPRO service.

**README.md** : This file, explaining how the application works.<br>
**.gitignore** : List of non-versioned files (such as the compiled binary).

After the compilation process, the non-versioned binary file `checkCpfC` will be generated.

### API Credentials

The application is configured with a personal TOKEN, please change it.<br>
Its possible to generate a new TOKEN on SERPRO service page: [servicos.serpro.gov.br](https://servicos.serpro.gov.br/inteligencia-de-negocios-serpro/biblioteca/consulta-cpf/teste.html).

The in use TOKEN is specified on the top of `middleware.h` file.<br>
If going to production the SERPRO service URL must also be changed (at the same file).

## Application Info

### Author

Heitor Peralles<br>
[heitorgp@gmail.com](mailto:heitorgp@gmail.com)<br>
[linkedin.com/in/heitorperalles](https://www.linkedin.com/in/heitorperalles)

### Source

[github.com/heitorperalles/checkCpfC](https://www.github.com/heitorperalles/checkCpfC)

### License

MIT licensed. See the **LICENSE** file for details.
