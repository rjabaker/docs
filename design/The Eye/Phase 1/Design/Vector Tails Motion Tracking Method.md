## Vector Tails Motion Tracking Method

Tracking joint motions with the Kinect camera is difficult because the camera has such a high frame rate, taking approximately thirty images per second. When comparing a joint's coordinates between two neighbouring frames, the coordinates are almost identical, even if the joint is moving fairly quickly. Therefore, a more effective motion tracking method needs to be devised.

The vector tails method requires a VectorTail object that is initialized with a maximum length: 

`new VectorTail(float maximumLength)`

A pseudo-joint object will be created, called a JointDisplacementTracker, which will contain a collection of VectorTail objects. This JointDisplacementTracker will take a maximum vector tail length and a maximum vector tail count as initialization parameters. It will also store a reference to its joint type.

`new JointDisplacementTracker(float maximumVectorTailLength, int maximumVectorTailCount, Microsoft.Kinect.JointType jointType)`

When the camera tracks motion, the application will pick out those joints it is currently tracking. It will then find the appropriate JointDisplacementTracker for that joint (there is one tracker object per joint). It will then send the tracker a reference to the joint, which contains the most updated coordinate. This will be in the TrackJointMethod in the JointDisplacementTracker.

`void TrackJointMethod(Microsoft.Kinect.Joint joint)`

This method will extract the joint's coordinate. It will then find the current (the incomplete) VectorTail object in its collection. This coordinate will be added to the VectorTail via the VectorTail method called AddCoordinate.

`AddCoordinate(Microsoft.Kinect.SkeletonPoint coordinate)`

This method will extend the VectorTail from its start coordinate to this new coordinate. If the VectorTail has no start, this coordinate will be its start. The VectorTail is a line vector. If the tail is equal to its maximum length, it will be marked as complete. The TrackJointMethod will then spawn a new VectorTail with no start point. If the tail is longer than the maximum length, the newly-spawned VectorTail will have an initial length equal to the excess length of the completed VectorTail. 

The number of existing VectorTails in the collection will be analyzed. If it exceeds the count specified in the tracker's constructor, the oldest tail will be discarded. 

The tracker will then determine, with the existing vector tail collection, if the joint has moved a sufficient amount. This section is still a work in progress...

Note that the VectorTail length determines the accuracy of the movements. Smaller lengths lead to more accurate movements.
