# Cell-Nuclei-analysis-Segmentation-using-watershed
Cell nuclei segmentation followed by counting and sizing the nuclei. The results are exported into a csv file for further analysis.

This code performs cell counting and size distribution analysis and dumps results into a csv file.
It uses watershed segmentationfor better segmentation.
Similar to grain analysis except here it segments cells. 

## Orignal Image

![Orignal Image](https://github.com/alam121/Cell-Nuclei-analysis-Segmentation-using-watershed/blob/master/Orignal%20Image%20.png)

## Implementaion

The first step is to use threshold function to covert our image in white as foreground and black as background.
The next step is to remove any white noise, this is done using morphological opening. To remove any small holes in the object, we can use morphological closing


So, now we know for sure that region near to center of objects are foreground and region much away from the object are background. Only region we are not sure is the boundary region of cells.

Next we need to create the markers for Water segmentation, for this we need to divide our image into sure foreground and background.

So we need to extract the area which we are sure they are cells. Using Erosion removes the boundary pixels, so whatever remaining, we can be sure it is cells. That would work if objects were not touching each other. But since they are touching each other, another good option would be to find the distance transform and apply a proper threshold. 

The **distance transform** is an operator normally only applied to binary images. The result of the transform is a graylevel image that looks similar to the input image, except that the graylevel intensities of points inside foreground regions are changed to show the distance to the closest boundary from each point

![Orignal Image](https://github.com/alam121/Cell-Nuclei-analysis-Segmentation-using-watershed/blob/master/distance.gif)



Next we need to find the area which we are sure they are not cells. For that, we dilate the result. Dilation increases object boundary to background. This way, we can make sure whatever region in background in result is really a background, since boundary region is removed. See the image below.


Now we know for sure which are region of cells, which are background and all. So we create marker (it is an array of same size as that of original image, but with int32 datatype) and label the regions inside it. The regions we know for sure (whether foreground or background) are labelled with any positive integers, but different integers, and the area we don’t know for sure are just left as zero. For this we use cv2.connectedComponents(). It labels background of the image with 0, then other objects are labelled with integers starting from 1.

But we know that if background is marked with 0, watershed will consider it as unknown area. So we want to mark it with different integer. Instead, we will mark unknown region, defined by unknown, with 0.


## Final result



![Segmented Results Image](https://github.com/alam121/Cell-Nuclei-analysis-Segmentation-using-watershed/blob/master/Segmented-Result.png)



