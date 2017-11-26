
"""
Mapping.py

Created by John Canty on 02-18-2017

This module is based on Peter Comb's original Python code for aligning two-color data

"""

import numpy as np
import matplotlib.pyplot as plt
import scipy.interpolate as interp

def makeLSQSpline( xl, yl, xr, yr, n):
    """ Creates fitting function for mapping the left channel onto the right channel """
    
    xmin = min(xl)
    xmax = max(xl)
    ymin = min(yl)
    ymax = max(yl)
    
    s = 0
    xknots = np.linspace(xmin+s, xmax+s, n)
    yknots = np.linspace(ymin-s, ymax+s, n)
    
    xspline = interp.LSQBivariateSpline(xl, yl, xr, xknots, yknots)
    yspline = interp.LSQBivariateSpline(xl, yl, yr, xknots, yknots)
    
    def mapping(xl, yl):
        xr = xspline.ev(xl,yl)
        yr = yspline.ev(xl,yl)
        return xr, yr
    
    mapping.xspline = xspline
    mapping.yspline = yspline
    mapping.maptype = "LSQspline"
    
    return mapping

def findpairs(coords,radius):
    '''Finds pairs of points in both channels
    
    Parameters:
    
    coords = numpy array of x and y coordinates from both
    the left and right images
    
    radius = distance that is used as error metric for determining
    pairs of points. Radius is determined based on the spacing of the grid 
    generated experimentally. For grid spacing of 0.5 um we will use an 
    error radius of 50 nm
    '''
    pairs = np.array([])
    coordsl = coords[0:2,:]
    coordsr = coords[2:4,:]
    
    # Calculate pairwise distance matrix between points of both channels
    for i in range(1,coordsl.shape[1]):
        for j in range(1,coordsr.shape[1]):
            xl = coordsl[0,:][i]
            yl = coordsl[1,:][i]
            xr = coordsr[0,:][j]
            yr = coordsr[1,:][j]
            
            # If distance within error, add to pair
            dist = np.sqrt((xl - xr)**2 +(yl - yr)**2)
            if dist < radius:
                pairs = np.concatenate((pairs, np.array([xl, yl, xr, yr])), axis=0)

    return pairs

