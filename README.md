
<!-- README.md is generated from README.Rmd. Please edit that file -->

# springer

> Sparse Group Variable Selection for Gene-Environment Interactions in the Longitudinal Study

Recently, regularized variable selection has emerged as a power tool to identify and dissect gene-environment interactions. Nevertheless, in longitudinal studies with high dimensional genetic factors, regularization methods for G×E interactions have not been systematically developed. In this package, we provide the implementation of sparse group variable selection, based on both the quadratic inference function (QIF) and generalized estimating equation (GEE), to accommodate the bi-level selection for longitudinal G×E studies with high dimensional genomic features. Alternative methods conducting only the group or individual level selection have also been included. The core modules of the package have been developed in C++.

## How to install

 - To install from github, run these two lines of code in R

<!-- end list -->

    install.packages("devtools")
    devtools::install_github("Yuwen-L/springer")

## Example

    #install.packages("devtools")
    #devtools::install_github("Yuwen-L/springer")
    library(springer)
    data("dat")
    e <- dat$e
    u=dim(e)[2]
    g <- dat$g
    y <- dat$y
    clin <- dat$clin
    if(is.null(clin)){t=0} else{t=dim(clin)[2]}
    beta0 <- dat$coef

    lambda1 = seq(0.01,0.1,length.out=2)
    lambda2 = seq(0.01,0.1,length.out=2)
    tunning = cv.springer(clin=NULL, e, g, y,beta0, lambda1, lambda2, nfolds=5, func="GEE", corr="independence", structure="bilevel", maxits=30, tol=0.1)
    lam1 <- tunning$lam1
    lam2 <- tunning$lam2
    lam1
    lam2
    tunning$CV
    
    beta = springer(clin=clin, e, g, y,beta0,func="GEE",corr="independence",structure="bilevel",
    lam1=dat$lam1, lam2=dat$lam2,maxits=30,tol=0.01)
    ##only focus on the genetic main effects and gene-environment interactions
    beta[1:(1+t+u)]=0
    ##effects that have nonzero coefficients
    pos = which(beta != 0)
    ##true positive and false positive
    tp = length(intersect(index, pos))
    fp = length(pos) - tp
    list(tp=tp, fp=fp)

## Methods

This package provides implementation for methods proposed in

  - Zhou, F., Lu, X., Ren, J., Ma, S. and Wu, C. (2020+). Sparse Group Variable Selection for Gene-Environment Interactions in the Longitudinal Study.
