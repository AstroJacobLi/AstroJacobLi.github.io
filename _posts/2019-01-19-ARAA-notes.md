---
layout: post
title:  "ARAA notes"
date:   2019-01-19 21:38:00
author: Jiaxuan Li
categories: paper
---
# ARAA notes
Feel very ashamed for the decadence in the past year 2018. I will try to read as many paper as possible and do my best in 2019.

---
## Deep learning and Astrophysics

### [The use of convolutional neural networks for modelling large optically-selected strong galaxy-lens samples](https://arxiv.org/pdf/1904.06199v1.pdf)

### [Photometric redshifts from SDSS images using a Convolutional Neural Network](https://arxiv.org/pdf/1806.06607.pdf)

### [The use of convolutional neural networks for modelling large optically-selected strong galaxy-lens samples](https://arxiv.org/abs/1904.06199v1)

---


## [Markov Chain Monte Carlo Methods for Bayesian Data Analysis in Astronomy](https://astrojacobli.github.io/Docs/MCMC-2017ARAA.pdf)


## [Weak Lensing for Precision Cosmology, by Rachel Mandelbaum](https://www.annualreviews.org/doi/abs/10.1146/annurev-astro-081817-051928)

### Moments of images
The moment of a 2-D distribution about the point $$(c_x, c_y)$$ is defined as a kind of average (this definition is straightforward):
\\[M_{m, n} = \frac{\int \rho(x, y) (x-c_x)^m (y-c_y)^n \md x\md y}{\int \rho(x, y)\md x\md y}.\\]

The moment can be generalized with a weighting function:
\\[M_{m, n} = \frac{\int I(x, y)W(x, y)( x-c_x)^m (y-c_y)^n \md x\md y}{\int I(x, y)W(x, y)\md x\md y},\\]
where $$I(x, y)$$ is the intensity map and $$W(x, y)$$ is a weighting function.

Since the images are discrete instead of continuous, we write the integral to summation:
\\[M_{m, n} = \frac{\sum_x \sum_y I(x, y)W(x, y)( x-c_x)^m (y-c_y)^n \md x\md y}{\sum_x \sum_y  I(x, y)W(x, y)\md x\md y}.\\]

The order of moment is defined as $$m+n$$. Here after we usually assume a unity weighting function.

- Zero order moment: $$M_{0, 0} = \sum_x \sum_y I(x, y)$$ is the total brightness of an image. We ignore the denominator here otherwise it will yield trivial result.
- First order moment: $$M_{1, 0} = \frac{\sum_x \sum_y I(x, y) x}{\sum_x \sum_y I(x, y)}$$ is the $$x$$ centroid of the image with an arbitrary coordinate origin. The centroid of image is $$(M_{1, 0}, M_{0, 1})$$. Given the centroid, we can remove the origin of image to the centroid, and calculate higher order moments.
{% include image.html url="/images/ellipse-moment.png" caption="Second order moments of ellipse" width=280 align="right" %}
- Second order moments: Here we discuss the second order moment of an ellipse with constance surface brightness $$\mu$$ (not in magnitude). Assume we have correct the coordinate origin to the center of this ellipse. The the horizontal aligned ellipse (black one in the right figure), it's straightforward to calculate its second order moments as:
\\[M_{2, 0} = \frac{1}{4}a^2,\ M_{0, 2} = \frac{1}{4}b^2,\ M_{1, 1} = 0.\\]
We already see that the second order moments contain information about the shape, size and orientation of an ellipse. Then we study a non-trivial case by rotating the ellipse by $$\theta$$ (pink ellipse). We are very familiar with the coordinate transformation between $$(x, y)$$ and $$(x', y')$$, which is written as
\\[x' = x\cos\theta - y\sin\theta,\ y' = x\sin\theta + y\cos\theta.\\]
Plug in these transformation and express $$M_{2, 0}'$$ using $$M_{2, 0},\ M_{0, 2},\ M_{1,1}$$, also noticing the Jacobian of this transformation is unity, we have:
\\[M_{2, 0}' = \frac{1}{4}(a^2\cos^2\theta + b^2\sin^2\theta),\\]
\\[M_{0, 2}' = \frac{1}{4}(a^2\sin^2\theta + b^2\cos^2\theta),\\]
\\[M_{1, 1}' = \frac{1}{4}(a^2 - b^2)\sin\theta\cos\theta.\\]
Then we can define a complex ellipticity which goes like
\\[\widetilde{e} = \frac{M_{2, 0}' - M_{0, 2}' + 2i M_{1, 1}'}{M_{2, 0}' + M_{0, 2}'}.\\]
And it turns out to be
\\[\widetilde{e} = \frac{1-q^2}{1+q^2}(\cos 2\theta + i\sin 2\theta),\\]
where $$q$$ is the axis ratio $$b/a$$, and $$ (1-q^2) / (1+q^2) = e$$ is the ellipticity of the ellipse. Thus
\\[\widetilde{e} = e_1 + i e_2 = e(\cos 2\theta + i\sin 2\theta).\\]
Hence we have many combinations to express the shape (ellipticity and position angle) and size: $$(\theta, e)$$, $$(e_1, e_2)$$, etc.

- Higher order moments tell us the distortion and anisotropy of the image. Using higher order moments, we can estimate the PSF structure and how well does the PSF model behave. 

### From Images to Catalogs
The signal of weak lensing is so weak that can be easily covered by all kinds of systematics. What observers need to do is to minimize these systematics. 
#### PSF modeling
The effective PSF is composed of atmospheric PSF, optical PSF, pixel response difference, etc (P.S. Rachel Mandelbaum is one of the authors of GalSim). The optical PSF can be generally modeled and is easier compared to atmospheric PSF.
- Why does PSF affect weak lensing? Weak lensing measures the shapes of (faint) galaxies, PSF can make the galaxy itself more circular, make the shape of outskirts irregular (due to PSF anisotropy), and also make deblending very hard.
- How to measure PSF? Generally we measure the profiles of stars in the field as PSF models, then interpolate (using regular polynomials or Chebyshev polynomials) these models to other positions where we concern. But interpolation often make sense only on single CCD level. PSF model behaves bad on the edges of CCDs (but can use dithering and PCA interpolation to mitigate).
- PSF estimated by stars might not be accurate for galaxies. PSF actually depends on the SED of objects, but stars and galaxies have different SEDs. This effect is especially severe for very-wide-band surveys (Euclid). Also the PSF estimated by bright stars are not suitable for galaxies (even not suitable for bright galaxies).

#### Detector Systematics
- Brighter-fatter effect. The electric field sourced by charges accumulated within a pixel can deflect the later coming electrons away, and the consequence is to enlarge the boundaries of the object. So we don't measure PSF from bright stars. Also saturation is an additional factor.
- Some other defects of detectors can also considered as systematics. The difference of pixel sized among all pixels results in astrometric and photometric errors. The responding of pixels may also correlated with galaxy shape and position. In the next generation near infrared survey (WFIRST), we need to mitigate systematics related to NIR detector defects (they are not CCDs).

#### Deblending
The final goal of deblending is to separate the lights from blended objects, and then measure the shapes. There are generally two kinds of blending. One is called 'recognized blending', in which you/algorithms can clearly detect two or more objects. The other is called 'unrecognized blending', in which you cannot tell things apart by eyes/algorithms easily. 
1. Deblending allow us to use larger sample, otherwise we have to exclude (e.g. CFHTLenS) those blended sources (which account for 50% in HSC).
2. Deblending allow us to give sources better photo-$$z$$. If two galaxies with different SED/redshift blend together and we consider them as one source, then the photo-$$z$$ can go weird. 

Now there are many deblending algorithms, some of them take the advantages of the state-of-art machine learning and deep learning techniques. It's easier to distinguish objects by combining multi-band images than just looking at single band. Peter Melchoir write the deblending package [`scarlet`](https://arxiv.org/abs/1802.10157) based on multi-band images. And guys at UCSC also try to deblend images using GAN (Generative Adversarial Networks) [&#x1f517;](https://arxiv.org/abs/1810.10098).

#### Image Combination
{% include image.html url="https://i.stack.imgur.com/B4hCQ.gif" caption="Nyquist Sampling Rate" width=300 align="right" %} 

We all know that longer exposure time gives better S/N image. So we stack single-CCD images together to get deeper detections. The stacked image is usually called 'coadd'. But 'coadd' is not only for getting deeper (which is usually the case for ground-based telescopes). The pixel sizes of space-based telescopes are mostly quite large, so the image is under-sampled. 
- What is Nyquist sample rate? For a sinusoidal time series with frequency $$f$$, if the sampling rate (frequency) is smaller than $$2f$$, you cannot get the correct frequency from the sampled data. Also if the sampling rate is below $$2f$$, you cannot distinguish between two frequencies since they behave similar under this sampling rate. 

This paper [&#x1f517;](http://adsabs.harvard.edu/abs/2011ApJ...741...46R) describes the obstacle of space telescopes whose pixel sizes are limited by some reasons. If the pixel size is small and also large FOV is wanted, then you will need more CCDs, thus a heavier satellite will be launched with much more money. For Euclid, the pixel size is larger than the Nyquist pixel size (which is determined by the resolution $$\lambda/D$$), thus the images are under-sampled. But this can be mitigated by dithering, which is also image combination.

For ground-based telescopes, there are basically two ways of using coadds. One is to do all science (measure flux, shape and estimate PSF) based on coadds. The other is to estimate PSF on the appropriate weighted combination of single-exposure PSF, but use coadds to measure properties of galaxies. This way doesn't waste the frame with best seeing.

"Representing the galaxy and PSF models as sums of Gaussians with different scale radii can drastically speed up the calculations."

#### Other Aspects of Image Processing
Sky subtraction always affect everything. Errors in sky subtraction can cause coherent problems with object detection, photometry, and shear estimation near very bright objects—bright stars or collections of bright galaxies (e.g., in galaxy clusters).



## [The Connection Between Galaxies and Their Dark Matter Halos, by Risa Wechsler and Jeremy Tinker](https://www.annualreviews.org/doi/abs/10.1146/annurev-astro-081817-051756)

## [The Faintest Dwarf Galaxies](https://arxiv.org/abs/1901.05465)

## [The Most Luminous Supernovae](https://arxiv.org/abs/1812.01428v1)

## [Gaussian Process in Machine Learning](http://www.gaussianprocess.org/gpml/chapters/RW.pdf)
