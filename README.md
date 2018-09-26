# Cosmic PCA
[![Licence](http://img.shields.io/badge/license-GPLv3-blue.svg?style=flat)](http://www.gnu.org/licenses/gpl-3.0.html)

The Transiting Exoplanet Survey Satellite (TESS) is expected to suffer from unusually bad cosmic ray contamination, thanks to its very deep pixels in comparison to Kepler. We want to see if we can solve this problem, and related problems in observational astronomy.

This is a fork of https://github.com/tjof2/robustpca, a C++ implementation of Robust Orthonormal Subspace Learning using the Armadillo 
linear algebra library, available under a GNU GPL v3.0 license, as accordingly is this code.

## Contents

+ [Description](#description)
+ [Installation](#installation)
+ [Usage](#usage)

## Description

We want to use [Robust PCA](https://statweb.stanford.edu/~candes/papers/RobustPCA.pdf) to separate a stream of TESS images into a sparse component (which hopefully tracks cosmic rays) and a low-rank component (which hopefully recovers the star field).

The Robust PCA implementation is taken from Tom Furnival's C++ implementation of the Robust Orthonormal Subspace Learning (ROSL) algorithm [1].

[1] X Shu, F Porikli, N Ahuja. (2014) "Robust Orthonormal Subspace Learning: Efficient Recovery of Corrupted Low-rank Matrices". ([paper](http://dx.doi.org/10.1109/CVPR.2014.495))<br/>

The cosmic ray generator is from the TESS Science Team [SPyFFi package](https://github.com/TESScience/SPyFFI). 

## Installation

**Dependencies**

This library makes use of the **[Armadillo](http://arma.sourceforge.net)** C++ linear algebra library, 
which needs to be installed first. It is recommended that you use a high-speed replacement for
LAPACK and BLAS such as OpenBLAS, MKL or ACML; more information can be found in the [Armadillo
FAQs](http://arma.sourceforge.net/faq.html#dependencies).

Also install SPyFFI from source at https://github.com/TESScience/SPyFFI or with

	pip install SPyFFI

**Building from source**

To build the library, unpack the source and `cd` into the unpacked directory, then type `make`:

```bash
$ tar -xzf robustpca.tar.gz
$ cd rosl
$ make
```

This will generate a C++ library called `librosl.so`, which is called by the Python module `pyrosl`.

## Usage

For a corrupted observation matrix **X**, one can run the ROSL algorithm with the following required
parameters:

```python
import pyrosl

example_rosl = pyrosl.ROSL( 
    method='full',
    rank = 5,
    reg = 0.1
)
example_rosl.fit_transform(X)

A = example_rosl.model_
E = example_rosl.residuals_

```

A simple Python script is included with examples of both ROSL and the fast ROSL+ algorithms, as well
as further optional parameters. It can be run on the command line with:

```bash
$ python example.py
```

Further documentation can be found in the file `pyrosl.py`.

## Todo

+ Write tests
