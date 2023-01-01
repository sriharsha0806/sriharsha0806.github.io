# Reading Notes

## Object Detection and Tracking 
**Publsihed** : Feb 11, 2020 
Object tracking has a wide range of applications in computer vision, such as serveillance, human-computer interaction, traffic flow monitoring, human activity recognition. 

### Simple Online and Realtime Tracking (sort)
[Simple Online and Realtime Tracking](https://arxiv.org/pdf/1602.00763.pdf) is a lean implementation of a tracking by detection framework. Keeping in line with Occam's Razor, it ignores appearance features beyond the detection component. SORT uses the position and size of the bounding boxes for both motion estimation and data association through frames. Faster RCNN is used as the object detector. The displacement of objects in the consecutive frames is estimated by a linear constant velocity model which is independent of other objects and camera motion. The state of each target is defined as x = [u, v, s, r, u', v', s'] where (u,v) represents the center of the bounding box, r and u indicate scale and aspect ratio. The other variables are the respective velocities. 

For the ID assignment, i.e., data association task the new target states are used to predict the bounding boxes that are later on compared with the detected boxes in the current timeframe. The IOU metric and the Hungarain algorithm are utilized for choosing the optimum box to pass on the identity. SORT achieves 74.6 MOTA and 76.9 IDF1 on the MOT17 dataset with 291 ID switches and 30 FPS.

Note: SORT fails to cope with occlusion 

### DeepSORT: 
In addition to that, DeepSORT adds extra dimensions to its motion tracking mode. The state of each target is denoted on the eight-dimensional state space (u, v, $/gamma$, h, x', y', $/gamma$', h') that contains the bounding box center position (u,v), aspect ratio $\gamma$, height h and their repective velocities in image coordinates. These additions enable DeepSORT to effectively handle challenging scenarios and reduce the number of identity switches by 45%. DeepSORT achieves 75.4 MOTA and 77.3 IDF1 on the MOT17 dataset with 239 ID switches but a lower FPS of 13. 

### FairMOT
Approaching object tracking from a multi-task learning perspective of object detection and re-ID in a single network is appealing since it allows shared optimization of the two tasks. However, the two tasks tend to compete with each other. In particular, previous works usually treat re-ID(re-Identification) as a secondary task whose accuracy is heavily affected by the primary detection task. As a result, the network is biased to the primary detection task which is not fair to the re-ID task.

![](https://media-exp1.licdn.com/dms/image/C5612AQHD_iagB0ffFQ/article-inline_image-shrink_1500_2232/0/1644494747437?e=1649894400&v=beta&t=z1dhE5UrrXul1LsxeTdwRyRSZRprVDeiu2c8ysFDgP4)

Comparison of the existing one-shot trackers and FairMOT. The three methods extract re-ID features differently. TrackR-CNN extracts re-ID features for all positive anchors using ROI-Align. JDE extracts re-ID features at the centers of all positive anchors. FairMOT extracts re-ID features at the object center

[FairMOT](https://arxiv.org/pdf/2004.01888v6.pdf) is a new tracking approach built on top of the anchor-free object detection architecture CenterNet. The detection and re-ID tasks are treated equally in FairMOT  which essentially differs from the previous "detention first, re-ID secondary"  frameworks. It has a simple network structure that consists of two homogeneous branches for detecting objects and extracting re-ID features. 

![](https://media-exp1.licdn.com/dms/image/C5612AQHtrj_1UtX8sQ/article-inline_image-shrink_1500_2232/0/1644494802457?e=1649894400&v=beta&t=Tiw8ghn-fHB6ZY3V5Gr85TFXODUzsiWlgmbfyeubShg)

### Overview of FairMOT and its encoder-decoder network
FairMOT adopts ResNet-34 as the backbone as it offers a good balance between accuracy and speed. An enhanced version of Deep Layer Aggression (DLA) is applied to the backbone to fuse multi-layer. In addition, convolution layers in all up-sampling modules are replaced by deformable convolutions so that they can dynamically adjust the receptive field according to object scales and poses. These modifications also help to solve the alignment issue. 

![](https://media-exp1.licdn.com/dms/image/C5612AQEd7ZXJ_FGA6w/article-inline_image-shrink_1500_2232/0/1644494842892?e=1649894400&v=beta&t=IQlCzflRKHoBRgjmx1FciLrJewjYlS0kHLTNy5gDxr8)

The detection branch is built on top of CenterNet, three parallel heads are appended to DLA-34 to estimate hearmaps, object center offsets, and bounding box sizes, respectively. The Re-ID branch aims to generate features that can distinguish objects. Ideally, affinity among different objects should be smaller than that between the same objects. To achieve this goal, FairMOT applies a convolution layer with 128 kernels on top of backbone features to extract re-ID features for each location. The re-ID features for each location. The re-ID features are learned through a classification task. All object instances of the same identity in the training set are treated as the same class. All these optimizations help FairMOT achieve 77.2 MOTA and 79.8 IDF1 on the MOT17 dataset with 25.9 FPS running speed. 

### TransMOT
Advances in deep learning have inspired us to learn spatial-temporal relationships using deep learning. Especially, the success of Transformer suggests a new paradigm of modeling temporal dependencies through the powerful self-attention mechanism. But transformer-based trackers have not been able to achieve state-of-the-art performance for several reasons:
* Videos can contain a large number of objects. Modeling the spatial-temporal relationships of these objects with a general transformer is inefficient because it does no take the spatial-temporal structure of the objects into consideration. 
* It requires a lot of computation resources and data for a transformer to model long-term temporal dependencies.
* The DETR-based object detector used in these works is still not state-of-the-art. 

![](https://media-exp1.licdn.com/dms/image/C5612AQFcTeXlwWvayw/article-inline_image-shrink_1500_2232/0/1644494963022?e=1649894400&v=beta&t=vtIJfCQDsVDJJnpoFNKuHcjq4BLDh6Z2-dCad5MC2xs)

#### Overview of TransMOT
[TransMOT](https://arxiv.org/pdf/2104.00194v2.pdf) is a new spatial-temporal graph Transformer that solves all these issues. It arranges the trajectories of all the tracked objects as a series of sparse weighted graphs that are constructed using the spatial relationships of the targets. TransMOT then uses these graphs to create a spatial graph transformer encoder layer, a temporal transformer encoder layer, and a spatial transformer decoder layer to model the spatial-temporal relationships of the objects. The sparsity of the weighted graph representations makes it more computationally efficient during training and inference. It is also a more effective model than a regular transformer because it exploits the structure of the objects. 

To further improve the tracking speed and accuracy, TransMOT introduces a cascade association framework to handle low-score detections and long-term occlusions. TransMOT can also be combined with different object detectors or visual feature extraction sub-networks to form a unified end-to-end solution. This enables developers to leverage state-of-the-art detectors for object tracking. TransMOT achieves 76.7 MOTA and 75.1 IDF1 on the MOT17 dataset with 9.6 FPS

### ByteTrack
Most tracking methods obtain identities by associating detection boxes with scores higher than a threshold. The objects with low detection scores are simply ignored, which brings non-negligible true object missing and fragmented trajectories. BYTE is an effective association method that utilizes all detection boxes from high scores to low ones in the matching process. 

![](https://media-exp1.licdn.com/dms/image/C5612AQFypRLG7fCQKA/article-inline_image-shrink_1500_2232/0/1644495019595?e=1649894400&v=beta&t=rdZDQ5tn7UJJBiQSRz-mkgtEEKMC3gyleCzlO8viSPo)

In frame t1, three different tracklets are initialized as their scores are all higher than 0.5. However, in frame t2 and frame t3 when occlusion happens, the red tracklet's corresponding detection score becomes lower i.e., 0.8 to 0.4 and then 0.4 to 0.1. These detection boxes are eliminated by the thresholding mechanism and the red tracklet disappears. On the other hand, if we take every detection box into consideration, more false positives will be introduced, for example, the right-most box in frame t3. 

![](https://media-exp1.licdn.com/dms/image/C5612AQFjeM2go5VPaw/article-inline_image-shrink_1500_2232/0/1644495070636?e=1649894400&v=beta&t=yNuIPdy2OwQDE-Gy9KUctJYn-1Gg3TCKOvJNdEQH5lg)

BYTE is built on the premise that the similarity with tracklets provide a strong cue to distinguish the objects and backgrounds in low score detection boxes. BYTE first matches the high score detection boxes to the tracklets based on motion similarity. It uses Kalman Filter to predict the location of the tracklets in the new frame. The motion similarity is computed by the IoU of the predicted box and the detection box. Then, it performs the second matching between the unmatched tracklets, i.e. the tracklet in the red box, and the low score detection boxes. As a result, the occluded person with low detection scores is matched correctly to the previous tracklet and the background is removed. 

|Tracker|MOTA|IDF1|FPS|
|--|--|--|--|
|SORT|74.6|46.9|30|
|DeepSORT|75.4|77.2|13|
|FairMOT|77.2|79.8|25.9|
|TransMOT| 76.7|75.1|9.6|
|BYTETrack|80.3|77.3|30|

Performances metrics for the trackers on the MOT17 benchmark

When applied to 9 different state-of-the-art trackers, BYTE achieves consistent improvement on IDF1 scores. To put forwards the state-of-the-art performance, BYTE was equipped with the high-performance detector [YOLOX](https://medium.com/augmented-startups/yolor-vs-yolox-battle-of-the-object-detection-prodigies-ae004a5ac8d2), to create a strong tracker named ByteTrack.