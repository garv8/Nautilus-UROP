![Logo](https://raw.githubusercontent.com/johannesulf/nautilus/main/docs/nautilus_text_image.png "Logo")

[![Unit Testing Status](https://img.shields.io/github/actions/workflow/status/johannesulf/nautilus/tests.yml?branch=main&label=tests)](https://github.com/johannesulf/nautilus/actions)
[![Documentation Status](https://img.shields.io/readthedocs/nautilus-sampler)](https://nautilus-sampler.readthedocs.io/en/latest/)
[![Code Coverage](https://img.shields.io/coverallsCoverage/github/johannesulf/nautilus)](https://coveralls.io/github/johannesulf/nautilus?branch=main)
[![PyPI](https://img.shields.io/pypi/v/nautilus-sampler)](https://pypi.org/project/nautilus-sampler/)
[![PyPI - Downloads](https://img.shields.io/pypi/dm/nautilus-sampler)](https://pypi.org/project/nautilus-sampler/)
[![Conda](https://img.shields.io/conda/vn/conda-forge/nautilus-sampler)](https://anaconda.org/conda-forge/nautilus-sampler)
[![Conda - Downloads](https://img.shields.io/conda/dn/conda-forge/nautilus-sampler)](https://anaconda.org/conda-forge/nautilus-sampler)
[![License: MIT](https://img.shields.io/github/license/johannesulf/nautilus)](https://raw.githubusercontent.com/johannesulf/nautilus/main/LICENSE)
![Language: Python](https://img.shields.io/github/languages/top/johannesulf/nautilus)

Nautilus is an MIT-licensed pure-Python package for Bayesian posterior and evidence estimation. It utilizes importance sampling and efficient space exploration using neural networks. Compared to traditional MCMC and Nested Sampling codes, it often needs fewer likelihood calls and produces much larger posterior samples. Additionally, nautilus is highly accurate and produces Bayesian evidence estimates with percent precision. I collaborated with Dr. Lange on this project in an attempt to improve the accuracy of the neural netwrok by leveraging TensorFlow. This version of the project has my TensorFlow code in nautilus/neural.py. This version had ~10% increase in accuracy when tested on the loggamma problem with 15+ dimensions.

[These were my findings](Urop_final.pdf), presented at the University of Michigan UROP symposium.

## Example

This example, sampling a 3-dimensional Gaussian, illustrates how to use nautilus.

```python
import corner
import numpy as np
from nautilus import Prior, Sampler
from scipy.stats import multivariate_normal

prior = Prior()
for key in 'abc':
    prior.add_parameter(key)

def likelihood(param_dict):
    x = [param_dict[key] for key in 'abc']
    return multivariate_normal.logpdf(x, mean=[0.4, 0.5, 0.6], cov=0.01)

sampler = Sampler(prior, likelihood)
sampler.run(verbose=True)
points, log_w, log_l = sampler.posterior()
corner.corner(points, weights=np.exp(log_w), labels='abc')
```

## Installation

The most recent stable version of nautilus is listed in the Python Package Index (PyPI) and can be installed via ``pip``.

```shell
pip install nautilus-sampler
```

Additionally, nautilus is also on conda-forge. To install via ``conda`` use the following command.

```shell
conda install -c conda-forge nautilus-sampler
```

## Documentation

You can find the documentation at [nautilus-sampler.readthedocs.io](https://nautilus-sampler.readthedocs.io).

## Attribution

A paper describing nautilus's underlying methods and performance has been accepted for publication. A draft of the paper is available on [arXiv](https://arxiv.org/abs/2306.16923). Please cite the paper if you find nautilus helpful in your research.

## License

Nautilus is licensed under the MIT License. The logo uses an image from the Illustris Collaboration.
