# fractal-interpolation
 (image)
 
Interpolating is used to make a continuous line from datapoints, in EO it can be used for along-track interpolation or image scaling.
The difference in image scales can often be a problem, as aligning two datasets requires them to be the same scale. Feature-matching algorithms are not always scale-invariant, as are many other algorithms. The task of increasing the scale is in principle the task of generating new points between original points, but in this case creating data does not create new information about the image detail, it is conserved. Depending on the specific use, the most common methods are linear and bicubic, in some cases edge-preserving algorithms which introduce artefacts. EO data often uses CNNs or Gaussian Processes (GP).
The results produced are mostly smooth, without artefacts. However, some artefacts can create details close to the ground truth, “cheating” the conservation of information. We can use fractal properties of nature to assume repeating patterns, making a better prediction about the in-between details than linear interpolation or GP.
There are numerous definitions of a fractal, but the one we need is its self-similarity across scales. The principle of a pattern repeating itself as we look closer is what we can use to interpolate between datapoints. 
The algorithm builds on the 1-d interpolation, which has a tuneable parameter SN.

(1-d image)

SN controls the level of noise added. Lower SN would result in more smooth, linear interpolation while increasing it would boost the amount of details.

The test image was taken directly from Copernicus S3 mission, a snapshot of Alpine winter due to the ractal nature of mountain ridges. 

To save calculation time for a 2-d image I made SN vary with each patch, so that more details would be added to the patches with edges or borders. SN linearly depends on the calculated patch entropy. While an edge-detection or contrast metric could be used to find patches with borders, fractal entropy is a more logical way to locate fractal borders. Fractal entropy equation is based on Shannon’s information entropy and is as cheap as contrast calculation. Patches are also made to overlap to avoid bordering.

(entropy image)

Gaussian Process alternative is introduced for comparison.

(result image)
