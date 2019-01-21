---
layout: post
title:  "ARAA notes"
date:   2019-01-19 21:38:00
author: Jiaxuan Li
categories: paper
---
# ARAA notes
Feel very ashamed for the decadence in the past year 2018. I will try to read as many paper as possible and do my best in 2019.

## [Weak Lensing for Precision Cosmology, by Rachel Mandelbaum](https://www.annualreviews.org/doi/abs/10.1146/annurev-astro-081817-051928)

### Moments of images
The moment of a 2-D distribution about the point $$(c_x, c_y)$$ is defined as a kind of average (this definition is straightforward):
\\[M_{m, n} = \frac{\int \rho(x, y) (x-c_x)^m (y-c_y)^n \md x\md y}{\int \rho(x, y)\md x\md y}.\\]

The moment can be generalized with a weighting function:
\\[M_{m, n} = \frac{\int I(x, y)W(x, y)( x-c_x)^m (y-c_y)^n \md x\md y}{\int I(x, y)W(x, y)\md x\md y},\\]
where $$I(x, y)$$ is the intensity map and $$W(x, y)$$ is a weighting function.

Since the images are discrete instead of continuous, we write the integral to summation:
\\[M_{m, n} = \frac{\sum_x \sum_y I(x, y)W(x, y)( x-c_x)^m (y-c_y)^n \md x\md y}{\sum_x \sum_y  I(x, y)W(x, y)\md x\md y}.\\]

The order of moment is defined as $$m+n$$.

- Zero order moment: $$M_{0, 0} = \sum_x \sum_y I(x, y)$$ is the total brightness of an image. We ignore the denominator here otherwise it will yield trivial result.

## [The Connection Between Galaxies and Their Dark Matter Halos, by Risa Wechsler and Jeremy Tinker](https://www.annualreviews.org/doi/abs/10.1146/annurev-astro-081817-051756)

## [The Faintest Dwarf Galaxies](https://arxiv.org/abs/1901.05465)

## [The Most Luminous Supernovae](https://arxiv.org/abs/1812.01428v1)

## [Gaussian Process in Machine Learning](http://www.gaussianprocess.org/gpml/chapters/RW.pdf)
