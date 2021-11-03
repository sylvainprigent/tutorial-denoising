Microscopy image denoising
==========================

This tutorial is an introduction to microscopy image denoising. It aims at guiding the more apropriate 
denoising method to the data type and the targeted application.

They are 3 main techniques of image denoising algorithms:

1. **Local filtering**: the local filtering is the most basic denoising technique. Each pixel is replaced with a combination of it neighbors. Typical values are the mean value, the median value, or a weighted average (ex: Gaussian filtering). 
2. **Variational denoising**: these methods are regularization based and aims at reconstructing a denoised image that looks alike the noisy image (ie small mean squared error) with a regularity constraint. The regularity constraint penalize the high variation considered as noise.
3. **Patch based denoising**: these method search similar patches in the image and filter them jointly. These methods are the more powerfull ones but have the drawback to introduce patches artefacts

Local Filtering
---------------

This toolbox implements the median filter in multi dimensions 2D, 3D, 3D+T.
The median filter has as many parameters than the image dimension. Each parameter is the radius of filter for a given dimension.

The drawback of local filtering is a blur effect proportional to the size of the filtering neighborhood. This method is then not recomended for a precise denoising but can be used as a preprocessing in a segmentation pipeline for example. 

*Example in 2D*:

For 2D filtering the two parameters are *rx*, the radius of the median filter in the X dimension and ry, the radius of the median filter in the Y dimension. The larger the filter, the stronger the denoising.


.. list-table:: 

    * - .. figure:: images/MAX_CELL01.png
           :scale: 75 %
           :alt: original image

           Original image

      - .. figure:: images/o_MAX_CELL01_median2d_3x3.png
           :scale: 75 %
           :alt: denoised image

           Median filtering with *sx=sy=3*

    * - .. figure:: images/o_MAX_CELL01_median2d_5x5.png
           :scale: 75 %
           :alt: denoised image

           Median filtering with *sx=sy=5*
           
      - .. figure:: images/o_MAX_CELL01_median2d_7x7.png
           :scale: 75 %
           :alt: denoised image

           Median filtering with *sx=sy=7*


Variational denoising
---------------------

Variational denoising is a familly of algorithm that reconstruct a new image that looks alike the noisy image 
with a smoothness constraint. It is this constraint that allows the image denoising. 

This toolbox implement the SPITFIR(e) denoising method [1] that regularize the image with the hessian (2nd order derivative) and the intensity of the image.
This constraint impose a smooth and sparse reconstructed image. 

Whatever the dimension (2D, 3D, 3D+t) the SPITFIR(e) alogrithm takes two main parameters.

1. *Regularization*: is a positive number. When this parameter increase, the influence of the regularization decrease. Thus if the regularization parameter is too low, the image is blurred, and if it is too high the image is not denoised.
2. *Weighting*: The weighting parameter controle the effect of the sparsity. We recommand 3 values. If the image is not sparse (ie: signal everywhere) set the sparsity to 0.9, if the image is sparse (ie. spot like structures) set the sparsity to 0.1, otherwise set the sparsity to 0.6

*Example in 2D*:

All the results above are obtained with a waighting parameter aquals to 0.6.

.. list-table:: 

    * - .. figure:: images/MAX_CELL01.png
           :scale: 75 %
           :alt: original image

           Original image

      - .. figure:: images/o_MAX_CELL01_spitfire2d_reg2.png
           :scale: 75 %
           :alt: denoised image

           SPITFIR(e) result with regularization=2

    * - .. figure:: images/o_MAX_CELL01_spitfire2d_reg5.png
           :scale: 75 %
           :alt: denoised image

           SPITFIR(e) result with regularization=5
           
      - .. figure:: images/o_MAX_CELL01_spitfire2d_reg10.png
           :scale: 75 %
           :alt: denoised image

           SPITFIR(e) result with regularization=10


Patch based denoising
---------------------

The patch based denoising methods allows to get a higer denoising effect but can generate artefacts.

This toolbox implements the NDSAFIR algorithm [2]. The main parameter is the size of the denoising patch in all the dimensions.

TODO: add here a desciption of all the parameter.

.. list-table:: 

    * - .. figure:: images/MAX_CELL01.png
           :scale: 75 %
           :alt: original image

           Original image

      - .. figure:: images/o_MAX_CELL01_safir2d_3x3_n2.png
           :scale: 75 %
           :alt: denoised image

           NDSAFIR result with path size 3x3

    * - .. figure:: images/o_MAX_CELL01_safir2d_5x5_n2.png
           :scale: 75 %
           :alt: denoised image

           NDSAFIR result with path size 5x5
           
      - .. figure:: images/o_MAX_CELL01_safir2d_7x7_n2.png
           :scale: 75 %
           :alt: denoised image

           NDSAFIR result with path size 7x7


[1] H. N. Nguyen, et al. SPITFIR(e): a supermaneuverable algorithm for 2D-3D+Time
fluorescence image restoration and background subtraction. BioRXiv, 2021

[2] J. Boulanger, et al. Patch-based non-local functional for denoising fluorescence microscopy image sequences. IEEE Trans. on Medical Imaging, 29(2): 442-454, 2010 
