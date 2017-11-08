[![Anaconda-Server Badge](https://anaconda.org/bioconda/sambamba/badges/installer/conda.svg)](https://conda.anaconda.org/bioconda)

[![DOI](https://zenodo.org/badge/doi/10.5281/zenodo.13200.svg)](http://dx.doi.org/10.5281/zenodo.13200)
# Sambamba

Sambamba is a high performance modern robust and fast tool (and
library), written in the D programming language, for working with SAM
and BAM files.  Current functionality is an important
subset of samtools functionality, including view, index, sort,
markdup, and depth. Most tools support piping: just specify `/dev/stdin`
or `/dev/stdout` as filenames.

For almost 5 years the main advantage over `samtools` was parallelized BAM reading.
Finally in March 2017 `samtools` 1.4 was released, reaching parity on this.
That said, we still have quite a few interesting features to offer:

- faster `sort` (no benchmarks yet, sorry)
- automatic index creation when writing any coordinate-sorted file
- `view -L <bed file>` utilizes BAM index to skip unrelated chunks
- `depth` allows to measure base, sliding window, or region coverages
  - [Chanjo](https://www.chanjo.co/) builds upon this and gets you to exon/gene levels of abstraction
- `markdup`, a fast implementation of Picard algorithm
- `slice` quickly extracts a region into a new file, tweaking only first/last chunks

Sambamba is free and open source software, licensed under GPLv2+.
See manual pages [online](https://lomereiter.github.io/sambamba/docs/sambamba-view.html)
to know more about what is available and how to use it.

For more information on Sambamba you can contact Artem Tarasov and Pjotr Prins.

# Binaries

With Conda use the [`bioconda`](https://bioconda.github.io/) channel.

A [GNU Guix package](https://www.gnu.org/software/guix/packages/s.html) for sambamba is available.

Debian: coming soon.

Users of Homebrew can also use the formula from `homebrew-science`.

For those not in the mood to learn/install new package managers, there are of course Github [releases](https://github.com/biod/sambamba/releases).

# Compiling Sambamba

The preferred method for compiling Sambamba is with the LDC compiler
which targets LLVM.

## Compilation dependencies

- zlib
- htslib (submodule)
- lz4 (submodule)
- BioD (submodule)
- undeaD (submodule)
- python2

## Compiling for Linux

The LDC compiler's github repository also provides binary images. The current
preferred release for sambamba is LDC - the LLVM D compiler (>= 1.1.0). After
installing LDC:

```sh
    git clone --recursive https://github.com/biod/sambamba.git
    cd sambamba
    git clone https://github.com/dlang/undeaD
    make sambamba-ldmd2-64
```

Installing LDC only means unpacking an archive and setting some environmental variables, e.g. unpacking into `$HOME`:

```sh
cd
wget https://github.com/ldc-developers/ldc/releases/download/v$ver/ldc2-$ver-linux-x86_64.tar.xz
tar xJf ldc2-$ver-linux-x86_64.tar.xz
export PATH=~/ldc2-$ver-linux-x86_64/bin/:$PATH
export LIBRARY_PATH=~/ldc2-$ver-linux-x86_64/lib/
```

### GNU Guix

To build sambamba the LDC compiler is also available in GNU Guix:

```sh
guix package -i ldc
```

## Compiling for Mac OS X

```sh
    brew install ldc
    git clone --recursive https://github.com/biod/sambamba.git
    cd sambamba
    git clone https://github.com/dlang/undeaD
    make sambamba-ldmd2-64
```

# Troubleshooting

In case of crashes it's helpful to have GDB stacktraces (`bt` command). A full stacktrace
for all threads:

```
thread apply all backtrace full
```

Note that GDB should be made aware of D garbage collector:
```
handle SIGUSR1 SIGUSR2 nostop noprint
```

A binary relocatable install of sambamba with debug information can be fetched from

```sh
wget http://biogems.info/contrib/genenetwork/s7l4l5jnrwvvyr3pva242yakvmbfpm06-sambamba-0.6.6-pre3-6ae174b-debug-x86_64.tar.bz2
md5sum ca64fd6f2fa2ba901937afc6b189e98d
mkdir tmp
tar xvjf ../*sambamba*.tar.bz2
cd tmp
```

unpack the tarball and run the contained install.sh script with TARGET

```
./install.sh ~/sambamba-test
```

Run sambamba in gdb with

```
gdb --args ~/sambamba-test/sambamba-*/bin/sambamba view --throw-error
```

# Getting help

Sambamba has a
[mailing list](https://groups.google.com/forum/#!forum/sambamba-discussion)
for installation help and general discussion.

## Reporting a sambamba bug or issue

Before posting an issue search the issue tracker and
[mailing list](https://groups.google.com/forum/#!forum/sambamba-discussion)
first. It is likely someone may have encountered something
similar. Also try running the latest version of sambamba to make sure
it has not been fixed already. Support/installation questions should
be aimed at the mailing list. The issue tracker is for development
issues around the software itself. When reporting an issue include the
output of the program and the contents of the .log.txt file in the
output directory.

## Check list:

1. [X] I have found and issue with sambamba
2. [ ] I have searched for it on the [issue tracker](https://github.com/biod/sambamba/issues) (also check closed issues)
3. [ ] I have searched for it on the [mailing list](https://groups.google.com/forum/#!forum/sambamba-discussion)
4. [ ] I have tried the latest [release](https://github.com/biod/sambamba/releases) of sambamba
5. [ ] I have read and agreed to below code of conduct
6. [ ] If it is a support/install question I have posted it to the [mailing list](https://groups.google.com/forum/#!forum/sambamba-discussion)
7. [ ] If it is software development related I have posted a new issue on the [issue tracker](https://github.com/biod/sambamba/issues) or added to an existing one
8. [ ] In the message I have included the output of my sambamba run
9. [ ] In the message I have included the relevant .log.txt file in the output directory
10. [ ] I have made available the data to reproduce the problem (optional)

To find bugs the sambamba software developers may ask to install a
development version of the software. They may also ask you for your
data and will treat it confidentially.  Please always remember that
sambamba is written and maintained by volunteers with good
intentions. Our time is valuable too. By helping us as much as
possible we can provide this tool for everyone to use.

## Code of conduct

By using sambamba and communicating with its communtity you implicitely
agree to abide by the
[code of conduct](https://software-carpentry.org/conduct/) as
published by the Software Carpentry initiative.

# Development

Sambamba development and issue tracker is on
[github](https://github.com/biod/sambamba). Developer
documentation can be found in the source code and the [development
documentation](https://github.com/biod/sambamba-dev-docs).

# Copyright

Sambamba is distributed under GNU Public License v2+.

# Credit

If you are using Sambamba in your research and want to support future
work on Sambamba, please cite the following publication:

A. Tarasov, A. J. Vilella, E. Cuppen, I. J. Nijman, and P. Prins. [Sambamba: fast processing of NGS alignment formats](https://doi.org/10.1093/bioinformatics/btv098). Bioinformatics, 2015.

```bibtex

@article{doi:10.1093/bioinformatics/btv098,
  author = {Tarasov, Artem and Vilella, Albert J. and Cuppen, Edwin and Nijman, Isaac J. and Prins, Pjotr},
  title = {Sambamba: fast processing of NGS alignment formats},
  journal = {Bioinformatics},
  volume = {31},
  number = {12},
  pages = {2032-2034},
  year = {2015},
  doi = {10.1093/bioinformatics/btv098},
  URL = { + http://dx.doi.org/10.1093/bioinformatics/btv098},
  eprint = {/oup/backfile/content_public/journal/bioinformatics/31/12/10.1093/bioinformatics/btv098/2/btv098.pdf}
```
