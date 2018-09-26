# Cosmic PCA
[![Licence](http://img.shields.io/badge/license-GPLv3-blue.svg?style=flat)](http://www.gnu.org/licenses/gpl-3.0.html)

The Transiting Exoplanet Survey Satellite (TESS) is expected to suffer from unusually bad cosmic ray contamination, thanks to its very deep pixels in comparison to Kepler. We want to see if we can solve this problem, and related problems in observational astronomy. Here, we use [Robust PCA](https://statweb.stanford.edu/~candes/papers/RobustPCA.pdf) (RPCA), which decomposes data into sparse and low-rank components, to separate out cosmic rays from stellar variability. To my [knowledge](https://ui.adsabs.harvard.edu/#search/q=abs%3A%22robust%20PCA%22%20database%3Aastronomy&sort=date%20desc%2C%20bibcode%20desc&p_=0), this technique has not so far been used for this purpose in astronomy, though in adjacent fields it has been used for [exoplanet direct imaging](https://ui.adsabs.harvard.edu/#abs/2016A&A...589A..54G/abstract), [baryons in haloes](https://ui.adsabs.harvard.edu/#abs/2014MNRAS.440..240D/abstract) and [geophysics](https://ui.adsabs.harvard.edu/#abs/2012GeoJI.190.1423S/abstract). 

This is a fork of https://github.com/tjof2/robustpca, a C++ implementation of Robust Orthonormal Subspace Learning, available under a GNU GPL v3.0 license, as accordingly is this code.

## Contents

+ [Description](#description)
+ [Installation](#installation)
+ [Usage](#usage)

## Description

We want to use Robust PCA to separate a stream of TESS images into a sparse component (which hopefully tracks cosmic rays) and a low-rank component (which hopefully recovers the star field).

The Robust PCA implementation is taken from Tom Furnival's C++ implementation of the [Robust Orthonormal Subspace Learning](http://dx.doi.org/10.1109/CVPR.2014.495) (ROSL) algorithm.

The cosmic ray generator is from the TESS Science Team [SPyFFi package](https://github.com/TESScience/SPyFFI). 

## Installation

**Dependencies**

This library makes use of the **[Armadillo](http://arma.sourceforge.net)** C++ linear algebra library, 
which needs to be installed first. 

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
loadings, components, E = full_rosl._fit(X)
loadings = loadings[:, :full_rosl.rank_]

model = np.dot(loadings, full_rosl.components_)
sparse = E.reshape(np.shape(dat)) # dat is input data in (ncad, nx, ny) format
lowrank = model.reshape(np.shape(dat))

```

A simple Python script is included with examples of both ROSL and the fast ROSL+ algorithms, as well
as further optional parameters. It can be run on the command line with:

```bash
$ python example.py
```

Further documentation can be found in the file `pyrosl.py`.

## Todo

+ Write tests
