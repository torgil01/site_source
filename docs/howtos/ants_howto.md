# ANTS Howto

## Apply forward transform

Inline math not working:$p(x|y) = \frac{p(y|x)p(x)}{p(y)}$, \(p(x|y) = \frac{p(y|x)p(x)}{p(y)}\).

also \$A =  B\$, ss 


$$
A \rightarrow  B
$$

and

$$

\begin{CD}
A @>a>> B\\
\end{CD}
$$


```
antsRegistrationSyNQuick.sh -d 3 \
		-f B.nii.gz \
		-m A.nii.gz \
		-o RegA2B
		
antsApplyTransforms -d 3 \
		-i A.nii.gz \
		-o ADeformed.nii.gz \
		-r B.nii.gz \
		-t RegA2B1Warp.nii.gz \
		-t RegA2B0GenericAffine.mat
```

## Apply inverse transform
Note reversed order and special notation for affine part!

```
antsRegistrationSyNQuick.sh -d 3 \
		-f B.nii.gz \
		-m A.nii.gz \
		-o RegA2B 
		
antsApplyTransforms -d 3 \
		-i B.nii.gz \
		-o BDeformed.nii.gz \
		-r A.nii.gz \
		-t [RegA2B0GenericAffine.mat,1] \
		-t RegA2B1InverseWarp.nii.gz
```


## Apply multiple transforms
Several transforms can be "stacked" in the call to 'antsApplyTransforms'. Transforms will be applied in reverse order of what is specified in the command. 
*Example*: A -> B + B -> C

```
antsApplyTransforms -d 3  \
		-i A.nii.gz  \
		-o A_to_C.nii.gz \
		-r C.nii.gz \
		-t BtoC_1Warp.nii.gz \
		-t BtoC_0GenericAffine.mat \
		-t AtoB_1Warp.nii.gz \
		-t AtoB_0GenericAffine.mat
```


## Full antsRegistation call

**Rigid body**
```   
antsRegistration --verbose 1\
    		 --dimensionality 3\
    		 --float 0\
    		 --output [${warpName}] \
    		 --interpolation Linear \
    		 --use-histogram-matching 0 \
    		 --winsorize-image-intensities [0.005,0.995]\
    		 --initial-moving-transform [${target},${moving},1]\
    		 --transform Rigid[0.1]\
    		 --metric MI[${target},${moving},1,32,Regular,0.25]\
    		 --convergence [1000x500x250x10,1e-6,10]\
    		 --shrink-factors 8x4x2x1\
    		 --smoothing-sigmas 3x2x1x0vox
```

**SyN**

````
antsRegistration --verbose 1\
		 --dimensionality 3\
		 --float 0\
		 --output [${warpName},${warpName}Warped.nii.gz,${warpName}InverseWarped.nii.gz]\
		 --interpolation Linear\
		 --use-histogram-matching 0\
		 --winsorize-image-intensities [0.005,0.995]\
		 --initial-moving-transform [${target},${moving},1]\
		 --transform Rigid[0.1]\
		 --metric MI[${target},${moving},1,32,Regular,0.25]\
		 --convergence [1000x500x250x10,1e-6,10]\
		 --shrink-factors 8x4x2x1\
		 --smoothing-sigmas 3x2x1x0vox\
		 --transform Affine[0.1]\
		 --metric MI[${target},${moving},1,32,Regular,0.25]\
		 --convergence [1000x500x250x10,1e-6,10]\
		 --shrink-factors 8x4x2x1\
		 --smoothing-sigmas 3x2x1x0vox\
		 --transform SyN[0.1,3,0]\
		 --metric MI[${target},${moving},1,32]\
		 --convergence [100x70x50x10,1e-6,10]\
		 --shrink-factors 8x4x2x1\
		 --smoothing-sigmas 3x2x1x0vox
````

## Resample image to some target image space (images are in register)
You don't have to produce the identity matrix externally. When you type theantsApplyTransforms command, simply use the word "identity" for your transform, i.e.
```
antsApplyTransforms ... -t identity
```
## Resample image to some target resolution (in mm)
```
ResampleImageBySpacing 3 input_image output_image x y z 0 0 0
```

## Combine several warps 
Although you can stack several transforms in "antsApplyTransforms", it may also be useful to combine several transforms in a warp file. 
```
antsApplyTransforms -d 3 \
		-i input -r ref \
		-n inter \
		-t warp2 \
		-t aff2 \
		-t warp1 \
		-t aff1 \
		-o [combinedWarp,1]
```

## ImageMath


## CreateWarpedGridImage

## ThresholdImage

## PrintHeader
```
Print image header.
```
