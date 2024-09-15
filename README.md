# football_computer_vision

This project shows the use of computer vision to perform a deeper level analysis of the DFL - Bundesliga Data Shootout dataset.  
Specifically the following tasks are performed on the dataset:

- **Object Detection**: Use YOLOv8 model for detecting players, referrees and ball
- **Tracking**: Track movement of the detected objects using ByteTrack
- **Team Classification**: Embedding analysis for players with SigLIP and UMAP. Then classify into different teams using K-Means Clustering
- **Keypoint Inference**: Detecting keypoint position in the field for real-time pitch understanding
- **Pitch Homography**: Virtual Lines and Field Overlay for homographic pitch projection
- **Ball movement analysis**: Top-Down projection for creating a tactical radar view for seeing the ball /players movement

Training details for models used in specific tasks:

- **Object Detection**
  - Used Pre-Trained YOLOv8 small model for fine-tuning on the DFL dataset to detect players, goalkeeper, referrees and ball
  - Goalkeepers are detected separate from players because they wear separate color jerseys
  - Original images which are 1920 X 1080 are subject to post-processing to rescale to 1280 X 1280
  - 400 images from the dataset are annotated with the required classes
  - Ball detection is challenging because of small size and cluttered background so appropriate images are selected so that the detector can locate the ball in challenging conditions
- **Tracking**
  - Track movement of the detected objects using ByteTrack  
- **Team Classification**
  - Embedding analysis for players with SigLIP and UMAP. Then classify into different teams using K-Means Clustering
- **Keypoint Inference**
  - Detecting keypoint position in the field for real-time pitch understanding
- **Pitch Homography**
  - Virtual Lines and Field Overlay for homographic pitch projection
- **Ball movement analysis**
  - Top-Down projection for creating a tactical radar view for seeing the ball /players movement
