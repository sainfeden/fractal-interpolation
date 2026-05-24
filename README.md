# Adaptive fractal interpolation for EO imagery upscaling

![](/bg.jpg) 
 
Interpolating is used to make a continuous line from datapoints, in EO it can be used for along-track interpolation or image scaling.
The difference in image scales can often be a problem, as aligning two datasets requires them to be the same scale. Feature-matching algorithms are not always scale-invariant, as are many other algorithms. The task of increasing the scale is in principle the task of generating new points between original points, but in this case creating data does not create new information about the image detail, it is conserved. Depending on the specific use, the most common methods are linear and bicubic, in some cases edge-preserving algorithms which introduce artefacts. EO data often uses CNNs or Gaussian Processes (GP).
The results produced are mostly smooth, without artefacts. However, some artefacts can create details close to the ground truth, “cheating” the conservation of information. We can use fractal properties of nature to assume repeating patterns, making a better prediction about the in-between details than linear interpolation or GP.
There are numerous definitions of a fractal, but the one we need is its self-similarity across scales. The principle of a pattern repeating itself as we look closer is what we can use to interpolate between datapoints [^1]. 

### 1D interpolation

The algorithm builds on the 1-d interpolation [^2], which has a tuneable parameter SN [^3].

![](/1d.png)

SN controls the level of noise added. Lower SN would result in more smooth, linear interpolation while increasing it would boost the amount of details.

The test image was taken directly from Copernicus S3 mission [^4], a snapshot of Alpine winter due to the ractal nature of mountain ridges. 

To save calculation time for a 2-d image I made SN vary with each patch, so that more details would be added to the patches with edges or borders. SN linearly depends on the calculated patch entropy. While an edge-detection or contrast metric could be used to find patches with borders, fractal entropy is a more logical way to locate fractal borders. Fractal entropy equation [^5] is based on Shannon’s information entropy and is as cheap as contrast calculation. Patches are also made to overlap to avoid bordering. Gaussian Process alternative is introduced for comparison.

![](/entropy.png)

### Results

![](/gp.png)

### Cropped

![](/crop.png)

### Successes 

Fractal-based interpolation helps to preserve details and structures gaussian or bilinear would otherwise smoothen. It does not rely on edges, making it more suitable for EO imagery. These details can be crucial for feature-matching algorithms, like SIFT and can help alignment to higher-resolution datasets.

### Shortcomings 

Boxiness - the algorithm repeats small pixel structures, creating boxy artefacts. Decreasing the scale of entropy patches helps to avoid that, but it stops the IFS from sensing larger-scale structures too.
Cost - As the algorithm processes each patch individually, it does not scale too well. If larger datasets would be used, this proof-of-concept method has to be optimized. Decreasing the patch size makes the computation faster. 
Use - This algorithm is not as versatile as others. For example, it does not make sense to use for built-up scenery, as there is hardly any fractality. Perhaps it can be mixed in with GP, enhancing only specific patches. 

### Environmental cost / impact

Laptop energy usage is on average 50W 
Depending on the settins algorithm takes approximately 15 minutes for fractal interpolation of a test image, 15 more for GP. 
Therefore, a usage for an image of a similar size and complexity takes 10-20Wh. 
Testing an algorithm took approximately 10 hours of pure computation, 500Wh.

The total calculated cost is 500Wh, ~ 2000 kJ, with each usage taking 40 kJ. Further optimisation can help decrease the cost.

### References 

[^1]: Barnsley, M.F. Fractal functions and interpolation. Constr. Approx 2, 303–329 (1986). https://doi.org/10.1007/BF01893434

[^2]: https://github.com/cjohnson318/fractal_interpolation

[^3]: Z. Shi, S. Yao, B. Li and Q. Cao, "A Novel Image Interpolation Technique Based on Fractal Theory," 2008 International Conference on Computer Science and Information Technology, Singapore, 2008, pp. 472-475, doi: 10.1109/ICCSIT.2008.185.
keywords: {Fractals;Pixel;Interpolation;Hardware;Geometry;Feathers;Mathematical model;Fractal interpolation;Image scaling;Fractal dimension}, 

[^4]: https://dataspace.copernicus.eu/

[^5]: Daza, A., Wagemakers, A., Georgeot, B. et al. Basin entropy: a new tool to analyze uncertainty in dynamical systems. Sci Rep 6, 31416 (2016). https://doi.org/10.1038/srep31416
