# Camera Pose Estimation

 - In this project, different camera pose estimation techniques namely 2D-2D epipolar constraint based method, 3D-2D Perspective-N-Point (PnP) method and 3D-3D Iterative Closest Point (ICP) method have been implemented and comparisons have been drawn on their accuracy and computation time.

 - A pair of RGBD image pairs have been used for this purpose.

## Implementation details

### Feature point extraction and matching

 - ORB feature points (Oriented FAST keypoints and Rotated BRIEF descriptors) were extracted. <br />
 - Matching was performed using bruteforce matcher. <br />

### 2D-2D Motion Estimation

 - The matched feature points were then used to find the Essential matrix using the RANSAC algorithm. <br />
 - Singular Value decomposition is performed on the essential matrix to obtain its orthogonal matrices and singular value matrix. <br />
 - From SVD, E = U$\sigma$V', and from epipolar constraints E = t^ R, where t and R are the tranlation vector and Rotation matrix to be determined.  <br />
 - Using the above equations we can determine R and t through U, V and $\sigma$. Four different solutions are obtained from which the only solution that gives positive depth value when tested with a candidate 2D point is chosen. <br />

### 3D-2D Motion Estimation

 - Feature matching is performed as earlier and the 3D points in frame 1 and its corresponding 2D point in frame 2 are used to obtain the transformation matrix T $\in$ SE(3) through optimization. <br />
 - Left perturbation model is used to determine the jacobian of the reprojection error(?) between the 3D-2D correspondences. <br />
 - Gauss-Newton optimization method was used to estimate the relative pose of the image pair. <br />
 - Robust Cholesky decomposition is used to solve the Normal equation calculated from the jacobians and residuals of each pair of points. <br />
 - Pose estimate is updated until there is a decrease in the cost function (L2 norm of the reprojection error). <br />
 - g2o library was also used to estimate the relative pose through Gauss-Newton optimization. <br />
 - It can be concluded that the native optimization implementation outperforms g2o by a factor of 10 in computation time and <Performance comparison>. <br />

### 3D-3D Motion Estimation

 - The matched feature points from earlier, along with the depth images and camera intrinsics were used to determine the 3D point positions of the pair of points. <br />
 - SVD based ICP was used to determine the relative pose of the image pair.  <br />
 - The centroid of all the 3D points in the two frames were determined, and the corresponding de-centroid points were formulated in both frames. <br />
 - SVD is applied on the matrix formed from the product of the de-centroid points in both frames, and the rotation matrix is obtained from the orthogonal matrices using which the tranlation vector is also determined.
 - The estimation was performed using non-linear optimization with the g2o library as well.


## Dependencies
```
Eigen
Sophus
OpenCV
g2o
Chrono
```