

## Dataset metrics

- 217k RGBD images
- 5884 different synthetic scenes
- 580 real world scenes
- 82 objects
- camera parameters (for point cloud generation)

## My Notes

- According to the labels
    - .heat map is stored in a “score_maps” folder in a .npz file
    - "heatmap": "score_maps/scene_3755/realsense/numpy/0008.npz",
- bbox_pj: list of lists
    - \[8 float followed by a string “objects”]

## Dataset Generation


We formulate the object finding as an object detection problem and associate 3 challenges with it in the bin picking context.

- 1: combining information from multiple modalities. RGB susceptible to shadow, depth is suscetible to reflectance and transparent objects.
- 2: scene understanding, occlusion, entanglement, which can often result in objects being detected multiple times
- 3: objects that change shape, spatial informationis not enough to separate instances

Multiple grippers exist, where no single gripper suitable for all type sof object. Finding an appropiate sequence for a given gripper can be complicated

- Step One: Custom Object Dataset and Novel Object Testset
    - Criteria: Parallel and vacuum grasp capability, transparency, reflectiveness, dimensions, industry/warehouse domain, deformability, texture, weight and fragility.
    - Novel object testset to test the grasping detection model on classes it has not seen before (without pre-existing 3D models) during training.
        - novel object have the following properties: convex/non-convex shape, transparency, varying shape, black color
- Step Two: Parallel-Jaw grasps sampling strategy and vacuum seal sampling strategy (skipped)
- Step Three: scene generation and object labels
    - let objects drop randomly into the bin
    - each scene is captured from 37 different camera viewpoints, with alternating light conditions.
    - grasp labels are assigned to each viewpoint
        - check parallel and vacuum suction grasps for visibility and collision with other objects or the tote when approaching the scene and performing the grasp
        - a good grasp depend on local object’s surface and the wrenches (external forces and torques) applied to the gripper contact.
        - A score is assigned to each of the 3 contact axes for the vacuum grasp
        - A score is assigned to each finger closing direction of the parallel jaw grasp
    - Object labels for each viewpoint
        - amodal segmentation
            - tuple of pixel-wise occlusion masks ($M_{occl.,k}$) for each object instance k in the scene, where the occlusion score [0,1] is then defined as occluded $M_{occl.,k}$ divided by total object suface area ($M_{total,k}=M_{occl.,k} \cup M_{vis.,k}$)
        - occlusion rate
        - semantic key points
            - joints or surface centers
            - perform ray tracing to check for visibility
            - datatype:tuple
            - (unique semantic id, image coordinate (x,y), class id, instance id)
        - center of mass distribution of heat maps
- Scene layout label and scene difficulties
    - Matrix storing the relation between each pair of objects, providing a comprehensive layout representation
    - Three types of relationship for a pair of objects:
        - 1 - A occludes B
        - 0 - No direct relationship
        - -1 - B occludes A
    - Based on 3 types of relationships we can create an NxN element relation matrix
    - Create a directed graph
        - top layer: objects clear of obstructions
        - second layer: objects covered by a single object
        - bottom layer: other objects
    - We label images according to 5 different levels of difficulty
![[Pasted image 20231106192156.png]]
	    - Based on 4 characteristics.
		- Number of layers  
		- occlusion percentage
		- instance completeness: refers to if a single object instance is visually crosscut into multiple segments due to occlusion
		- class uniqueness: do all objects in an image belong to different categories or are visually distinct from one another
	- First two levels concerned with layers and occlusions
	- Last 3 levels measure models ability to label object instances
	- level 1 minimal occlusion
	- level 2 some occlusion
	- level 3 includes incomplete objects
	- level 4 includes non-unique objects
	- level 5 includes incomplete and unique objects