# Reading Notes

## Object Detection and Tracking 
**Publsihed** : Feb 11, 2020 
Object tracking has a wide range of applications in computer vision, such as serveillance, human-computer interaction, traffic flow monitoring, human activity recognition. 

### Simple Online and Realtime Tracking (sort)
[Simple Online and Realtime Tracking](https://arxiv.org/pdf/1602.00763.pdf) is a lean implementation of a tracking by detection framework. Keeping in line with Occam's Razor, it ignores appearance features beyond the detection component. SORT uses the position and size of the bounding boxes for both motion estimation and data association through frames. Faster RCNN is used as the object detector. The displacement of objects in the consecutive frames is estimated by a linear constant velocity model which is independent of other objects and camera motion. The state of each target is defined as x = [u, v, s, r, u', v', s'] where (u,v) represents the center of the bounding box, r and u indicate scale and aspect ratio. The other variables are the respective velocities. 

For the ID assignment, i.e., data association task the new target states are used to predict the bounding boxes that are later on compared with the detected boxes in the current timeframe. The IOU metric and the Hungarain algorithm are utilized for choosing the optimum box to pass on the identity. SORT achieves 74.6 MOTA and 76.9 IDF1 on the MOT17 dataset with 291 ID switches and 30 FPS.

Note: SORT fails to cope with occlusion 

### DeepSORT: 
In addition to that, DeepSORT adds extra dimensions to its motion tracking mode. The state of each target is denoted on the eight-dimensional state space (u, v, $/gamma$, h, x', y', $/gamma$', h') that contains the bounding box center position (u,v), aspect ratio $/gamma$, height h and their repective velocities in image coordinates. These additions enable DeepSORT to effectively handle challenging scenarios and reduce the number of identity switches by 45%. DeepSORT achieves 75.4 MOTA and 77.3 IDF1 on the MOT17 dataset with 239 ID switches but a lower FPS of 13. 

### FairMOT
Approaching object tracking from a multi-task learning perspective of object detection and re-ID in a single network is appealing since it allows shared optimization of the two tasks. 

![](https://media-exp1.licdn.com/dms/image/C5612AQHD_iagB0ffFQ/article-inline_image-shrink_1500_2232/0/1644494747437?e=1649894400&v=beta&t=z1dhE5UrrXul1LsxeTdwRyRSZRprVDeiu2c8ysFDgP4)
