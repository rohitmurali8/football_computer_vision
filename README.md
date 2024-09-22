# football_computer_vision

This project shows the use of computer vision to perform a deeper level analysis of the DFL - Bundesliga Data Shootout dataset.  
Specifically the following tasks are performed on the dataset:

- **Object Detection**: Use YOLOv8 model for detecting players, referrees and ball
- **Tracking**: Track movement of the detected objects using ByteTrack
- **Team Classification**: Embedding analysis for players with SigLIP and UMAP. Then classify into different teams using K-Means Clustering
- **Keypoint Inference**: Detecting keypoint position in the field for real-time pitch understanding
- **Pitch Homography**: Virtual Lines and Field Overlay for homographic pitch projection. Top-Down projection for creating a tactical radar view for seeing the ball /players movement

NOTE - Most of the visualizations are done using the *supervision* package which makes it very easy to visualize boxes, keypoints, tracking path, etc 

Training details for models used in specific tasks:

- **Object Detection**
  - Used Pre-Trained YOLOv8 small model for fine-tuning on the DFL dataset to detect players, goalkeeper, referrees and ball
  - Goalkeepers are detected separate from players because they wear separate color jerseys
  - Original images which are 1920 X 1080 are subject to post-processing to rescale to 1280 X 1280
  - 400 images from the dataset are annotated with the required classes
  - Ball detection is challenging because of small size and cluttered background so appropriate images are selected so that the detector can locate the ball in challenging conditions
  - This is also the reason why the images are resized to 1280 X 1280 and not 640 X 640 because at 640 X 640 the ball will have very few pixels on it for accurate detection
- **Tracking**
  - Track movement of the detected objects using ByteTrack  
- **Team Classification**
  - The detected players with the YOLOv8 model are cropped into numpy arrays, pre-processed and passed into a pre-trained SigLIP model in HuggingFace
  - This SigLIP model gives us the embeddings which are 768 dimensions in size. The embeddings are then scaled down to 3 dimensions using UMAP 
  - The reduced embeddings are then classified into 2 clusters using K-Means Clustering to represent 2 different teams
- **Keypoint Inference**
  - Detecting keypoint position in the field for real-time pitch understanding
  - 32 points across the pitch are annotated as keypoints. These 32 points include the goal area, center of the field, penalty spots, corners, etc
  - After annotating the points on the field, the images are resized to 640 X 640
  - The annotated images are downloaded from roboflow which also contain a data.yaml file which contains the index mapping between keypoints in case the image is flipped for augmentations
  - The model is trained for around 100 epochs so that the predicted keypoints are accurate 
- **Pitch Homography**
  - Homography helps in projecting points from one plane onto another plane by using a 3X3 matrix
  - For homographic projection, we need atleast 4 pairs of points between the source and target plane
  - The target plane in this case is an image that contains real-world information about the geometry of the soccer pitch. This image is accessed from the SoccerConfigurationPitch class of the Roboflow sports repository
  - The source plane points are the keypoints which are detected by the keypoint detection model trained. The homography is calculated using the cv2.findHomography function
  - Once the homography is estimated, the points from the soccer pitch configuration are projected onto the images used in training to compare with the detected keypoints from the model
  - The keypoints detected using the model are also projected onto the configuration pitch to have a top-down view of the detected players, referees, ball, etc 
