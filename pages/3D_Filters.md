---
autogenerated: true
title: 3D Filters
breadcrumb: 3D Filters
layout: page
author: test author
categories: 
description: test description
---

## 3D Filters

3D filtering using ellipsoidal neighbourhood, as part of the [3D ImageJ Suite](3D_ImageJ_Suite "wikilink").

## Author

{% include person content='Tboudier' %}

Matthias Labschütz contributed a fast version for isotropic kernels (for min, max and mean filtering)

## Description

The plugin performs 3D filtering on image stacks. The kernel is ellipsoidal, you can then select three radii for X, Y, and Z. Note that the plugin uses parallel implementation, and a very fast version using //rolling ball// algorithm is used for isotropic kernel (all three radii equal).

The 3D filters mean, median, variance, min and max are implemented in ImageJ since version 1.47.

Available filters :

  - Mean and variance filtering

<!-- end list -->

  - Minimum and maximum filtering

<!-- end list -->

  - Local maxima

<!-- end list -->

  - TopHat transform (to detect spots-like objects)

<!-- end list -->

  - Sobel (simple edge detection)

<!-- end list -->

  - Adaptive Nagao-like

![3d\_variance.png](/images/pages/3d_variance.png "3d_variance.png")

3D variance

## Download

For details go to [3D ImageJ Suite](3D_ImageJ_Suite "wikilink").

If you want fast version using GPU, please refer to [CLIJ](CLIJ "wikilink").

## Citation

When using the *3D Filters* plugin for publication, please refer to :

J. Ollion, J. Cochennec, F. Loll, C. Escudé, T. Boudier. (**2013**) TANGO: A Generic Tool for High-throughput 3D Image Analysis for Studying Nuclear Organization. *Bioinformatics* 2013 Jul 15;29(14):1840-1. [doi](http://dx.doi.org/10.1093/bioinformatics/btt276)

## License

GPL distribution (see [license](http://www.cecill.info/index.en.html)).

## Changelog

  - 10/04/2017 : new timer for computing time
  - 28/05/2013 : new filters Open and Close
  - 22/10/2012 : bug fixed for filters3D with 32-bits images
  - 22/06/2011 : Parallelization updated

## Known Bugs

  - ij1.47d : Bug in Color/Split, the resulting stacks have incorrect bitdepth