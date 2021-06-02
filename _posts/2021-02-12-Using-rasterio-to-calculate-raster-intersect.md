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
{% gist 1fe456896b16ef90164d47aa8a62205b %}
