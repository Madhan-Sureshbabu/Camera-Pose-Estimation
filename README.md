# Camera Pose Estimation

 - In this project, different camera pose estimation techniques namely 2D-2D epipolar constraint based method, 3D-2D Perspective-N-Point (PnP) method and 3D-3D Iterative Closest Point (ICP) method have been implemented and comparisons have been drawn on their accuracy and computation time.

 - A pair of RGBD image pairs have been used for this purpose.

## Implementation details

### Feature point extraction and matching

 - ORB feature points (Oriented FAST keypoints and Rotated BRIEF descriptors) were extracted. <br />
 - Matching was performed using the bruteforce matcher. <br />

### 2D-2D Motion Estimation

 - The matched feature points were then used to find the Essential matrix(E) using the RANSAC algorithm. <br />
 - Singular Value Decomposition was performed on the essential matrix to obtain its orthogonal matrices and singular value matrix. <br />
 - From SVD, E = UΣV', and from the epipolar constraints, E = t^ R, where t and R are the tranlation vector and rotation matrix to be estimated.  <br />
 - Using the above relations, R and t were obtained using U, V and Σ. Four different solutions are obtained from which the only solution that gives positive depth value when tested with a candidate point is chosen as the essential matrix. <br />

### 3D-2D Motion Estimation

 - Using the feature correspondences from above, the 3D points in frame 1 and its corresponding 2D point in frame 2 were used to estimate the transformation matrix T ∈ SE(3) through non-linear optimization. <br />
 - Left perturbation model was used to determine the jacobian of the reprojection error between the 3D-2D point correspondences with respect to the relative pose update expressed in lie algebra ξ ∈ se(3). <br />
 - Gauss-Newton optimization method was implemented and Robust Cholesky decomposition was used to solve the obtained normal equation calculated from the jacobians and residuals of each pair of points. <br />
 - L2 norm was used as the loss function and the pose estimate was updated until the cost function decreased. <br />
 - g2o was also used to estimate the relative pose through Gauss-Newton optimization to verify the results from above.
 <!-- - It can be concluded that the native optimization implementation outperforms g2o by a factor of 10 in computation time and <Performance comparison>. <br /> -->

### 3D-3D Motion Estimation

 - The matched feature points from earlier, along with the depth images and camera intrinsics were used to determine the 3D positions of all points. <br />
 - SVD based ICP was used to estimate the relative pose of the image pair. The centroid of all the 3D points in the two coordinate frames were determined, and the corresponding de-centroid points were calculated for this purpose. <br />
 - The above result was also verified using estimates from Levenberg–Marquardt algorithm implemented with g2o.


## Dependencies
```
Eigen
Sophus
OpenCV
g2o
Chrono
```