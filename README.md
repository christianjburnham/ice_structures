# ice_structures
coordinates for ice structure minima

Each leaf-directory contains a file called 'monte_min.xyz'.

The structure of this file is as follows:

First line: number of nuclear sites, nsites
Second line: ANGLE (keyword used by our code) followed by 6 real numbers: amag, bmag, cmag, alpha, beta, gamma, specifying the cell vector dimensions
       in the normal crystallographic convention, followed by one real number giving the energy in kJ/mol
Next nsites lines:
atomic name, followed by 3 real numbers: x, y, z specifying the coordinates of that particle in Angstroms.

------------------------

Users may want to convert from amag,bmag,cmag,alpha,beta,gamma to cell vectors in Cartesian form. A snippet of FORTRAN code for doing so is given here:

!     convert alpha,beta,gamma to radians

      pi = 4.0d0 * datan(1.0d0)
      angfac = 180.0d0 / pi

      alpha_rad = alpha / angfac
      beta_rad = beta / angfac
      gamma_rad = gamma / angfac

!     cvec is a 3x3 array which holds the Cartesian values of the cell-vectors

      cvec = 0.0d0

      cvec(1,1) = amag
      cvec(1,2) = bmag * dcos(gamma_rad)
      cvec(2,2) = dsqrt(bmag**2 - cvec(1,2)**2)
      cvec(1,3) = cmag * dcos(beta_rad)
      cvec(2,3) = (bmag * cmag * dcos(alpha_rad) - cvec(1,2) * cvec(1,3))/cvec(2,2)
      cvec(3,3) = dsqrt(cmag**2 - cvec(1,3)**2 - cvec(2,3)**2)

!     or in terms of Cartesian components of a, b and c

      ax = cvec(1,1)
      ay = 0.0d0
      az = 0.0d0

      bx = cvec(1,2)
      by = cvec(2,2)
      bz = 0.0d0

      cx = cvec(1,3)
      cy = cvec(2,3)
      cz = cvec(3,3)
