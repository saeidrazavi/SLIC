# SLIC

<table>
  <tr>
    <td>Original Image</td>
     <td>1024 cluster</td>

  </tr>
  <tr>
    <td><img src= "https://github.com/saeidrazavi/SLIC/assets/67091916/e1913720-0293-4419-80b9-70135eca7ed3.jpg" width=500 height=200></td>
    <td><img src="https://github.com/saeidrazavi/SLIC/assets/67091916/3a114851-d70a-4ea2-8081-8282012b2adc.jpg" width=500 height=200></td>
  </tr>
 </table>
The SLIC algorithm is used to divide an image into superpixels. The algorithm works by initializing cluster centers and then perturbing them based on the gradient of the image in the LAB color space. After perturbing the cluster centers, the algorithm calculates the total distance for all the pixels, which is defined as $total-distance=d_{lab}+\alpha d_{xy}$, where $d_{lab}$ is the distance between LAB values of a pixel and $d_{xy}$ is the Euclidean distance between pixels. The algorithm then relates a cluster to each pixel, similar to k-means segmentation, and repeats this procedure until the clusters for each pixel do not change anymore. It is important to optimize the method by vectoring instead of using for loops, for which np.indices can be used to speed up the code for calculating $d_{xy}$. The algorithm can be summarized as follows:

* Initialize cluster centers.

* Perturb the cluster centers based on the image gradient.

* Calculate the total distance for all pixels.

* Relate a cluster to each pixel.

* Repeat until the clusters for each pixel do not change anymore.

The algorithm involves various steps, such as finding the gradient of the image, calculating the Euclidean distance, and determining the LAB distance. These steps are essential for the proper implementation of the SLIC algorithm.


## Finding the Gradient of the Image

```python
def gradients(mat):
    result = np.array(ndimage.sobel(mat))
    return result
```

## Calculating Euclidean Distance

```python
def euclidean_distance(x: np.ndarray, y: np.ndarray, x_i, y_i):
    dxy = np.sqrt((x-x_i)**2 + (y-y_i)**2)
    return dxy
```

## Find Boundaries

```python

def borders(score_mat, imagee):

    scores = scipy.signal.medfilt2d(score_mat[:, :], 29)

    borders = skimage.segmentation.find_boundaries(
        np.uint8(scores), mode='outer', connectivity=0.5).astype(bool)

    x, y = np.where(borders == 1)
    img = np.copy(imagee)
    img[x, y, :] = 0

    return img
```

## Segmentation and Boundary Detection

```python
def slic_seg(x, y, k, s, image: np.ndarray, lab_image: np.ndarray):
    # Implementation of segmentation
      lab_temp = lab_image[max(
            i-s, 0):min(i+s, c1), max(j-s, 0):min(j+s, c2), :]

        lab_dis = lab_distance(
            lab_temp, lab_image[i, j, 0], lab_image[i, j, 1], lab_image[i, j, 2])
        # ------------
        xy_dis = euclidean_distance(
            i_mat[max(i-s, 0):min(i+s, c1), max(j-s, 0):min(j+s, c2)], j_mat[max(i-s, 0):min(i+s, c1), max(j-s, 0):min(j+s, c2)], i, j)
        
        # ...

    return scores
```

## Results

The SLIC algorithm can be used to achieve image segmentation and superpixel generation. The proper implementation of the algorithm involves the steps mentioned above and requires careful consideration of the parameters involved, such as the number of clusters and the sensitivity parameter ($\alpha$). For more details and the complete implementation, please refer to the provided code file. 
<table>
  <tr>
    <td>Original Image</td>
     <td>256 cluster</td>
     <td> 1024cluster</td>
      <td>2048 cluster</td>

  </tr>
  <tr>
    <td><img src= "https://github.com/saeidrazavi/SLIC/assets/67091916/e1913720-0293-4419-80b9-70135eca7ed3.jpg" width=300 height=200></td>
    <td><img src="https://github.com/saeidrazavi/SLIC/assets/67091916/aec8d247-d288-44da-89e3-2c7ddb6f5a5d.jpg" width=300 height=200></td>
    <td><img src="https://github.com/saeidrazavi/SLIC/assets/67091916/06df3e14-1ad1-4941-8a20-6768883efc87.jpg" width=300 height=200></td>
    <td><img src="https://github.com/saeidrazavi/SLIC/assets/67091916/3a114851-d70a-4ea2-8081-8282012b2adc.jpg" width=300 height=200></td>
  </tr>
 </table>


