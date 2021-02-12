---
title: "Calculate intersect of rasters using RasterIO in Python"
#description: "Awesome description"
layout: post
toc: false
comments: true
#image: images/some_folder/your_image.png
hide: false
search_exclude: true
categories: [gis, python, rasterio]
---
# The problem
While doing some work on a machine learning application, I needed to create image chips from two raster images for inference.
My goal was to find the intersect to minimize exporting chips that had no data in the corresponding chip.
I started by manually calculating the intersect from the bounds and determining the pixel that corresponded to the location.
Although this method worked on my test cases, I was skeptical it would work on all inputs. I eventually rooted around long enough
and found the following solution to be much more elegant.

# The solution
'''
import rasterio

def get_intersect(image_one, image_two):

    """
    Computes intersect of input two rasters.
    :param image_one: first image
    :param image_two: second image
    :return: tuple of intersect in (left, bottom, right, top)
    """

    with rasterio.open(image_one) as one:
        pre_win = rasterio.windows.Window(0, 0, one.width, one.height)
        pre_bounds = one.window_bounds(pre_win)

    with rasterio.open(image_two) as two:
        post_win = rasterio.windows.Window(0, 0, two.width, two.height)
        pre_win_bounds = two.window(*pre_bounds)
        assert rasterio.windows.intersect(pre_win_bounds, post_win)
        intersect_win = post_win.intersection(pre_win_bounds)

        return two.window_bounds(intersect_win)

bounds = get_intersect('file_one', 'file_two')
'''