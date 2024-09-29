

# Image Stitching and Object Segmentation

## Table of Contents
- [Installation](#installation)
- [Project Structure](#project-structure)
- [Features](#features)
  - [1. Video Frame Analysis](#1-video-frame-analysis)
  - [2. Image Stitching and Panorama Creation](#2-image-stitching-and-panorama-creation)
- [Detailed Function Descriptions](#detailed-function-descriptions)
  - [Video Frame Analysis Functions](#video-frame-analysis-functions)
  - [Image Stitching Functions](#image-stitching-functions)
- [Output](#output)
- [Justification for SIFT](#justification-for-sift)
- [Panoramic Mosaicing Explanation](#panoramic-mosaicing-explanation)
- [References](#references)

## Installation

To run the code, ensure you have the following installed:

- Python 3.x
- OpenCV (`cv2`)
- NumPy
- Matplotlib
- Google Colab (for running the code in the cloud environment)
- Google Colab patches for image display

```bash
pip install opencv-python numpy matplotlib
```

## Project Structure

The project consists of the following main parts:

1. **Video Frame Analysis:**
   - Detects blurry frames in a video.
   - Segments background and detects edges in each frame.
   - Detects and filters lines, computes intersections, and overlays the information on frames.

2. **Image Stitching and Panorama Creation:**
   - Extracts features using SIFT.
   - Matches features using FLANN-based matcher.
   - Creates a panoramic mosaic from multiple images.

## Features

### 1. Video Frame Analysis

The project processes each frame of a video to:

- **Detect Blurry Frames:** Uses a custom kernel to detect blurriness in frames.
- **Segment Background:** Segments the background based on a threshold value.
- **Detect Edges:** Uses Canny edge detection.
- **Detect and Filter Lines:** Extracts lines using the Hough Transform and filters out short lines.
- **Compute Intersections:** Calculates intersections of lines to identify corners.
- **Overlay Lines and Corners:** Overlays detected lines and corners on frames.

### 2. Image Stitching and Panorama Creation

The project stitches a set of images to create a panoramic view:

- **Feature Extraction:** Uses SIFT to detect and describe features in images.
- **Feature Matching:** Matches features between consecutive images using FLANN-based matcher.
- **Combine Images:** Stitches images together using homography to create a seamless panorama.

## Detailed Function Descriptions

### Video Frame Analysis Functions

1. **`extract_frames(video_path)`**
   - Extracts and returns all frames from the provided video path.
   - **Parameters:**
     - `video_path` (str): Path to the video file.
   - **Returns:**
     - `frames` (list): List of frames extracted from the video.

2. **`is_blurry(frame, threshold=50)`**
   - Checks if a frame is blurry based on variance of the custom kernel.
   - **Parameters:**
     - `frame` (np.array): The frame to be analyzed.
     - `threshold` (int): Threshold variance value for determining blurriness.
   - **Returns:**
     - `bool`: True if the frame is blurry, otherwise False.

3. **`segment_background(frame)`**
   - Segments the background of a frame using a binary threshold.
   - **Parameters:**
     - `frame` (np.array): The frame to be processed.
   - **Returns:**
     - `thresh` (np.array): Binary segmented image.

4. **`detect_edges(frame)`**
   - Detects edges using the Canny algorithm.
   - **Parameters:**
     - `frame` (np.array): The frame for edge detection.
   - **Returns:**
     - `edges` (np.array): Edge-detected image.

5. **`filter_lines(lines)`**
   - Filters out short lines from a list of detected lines.
   - **Parameters:**
     - `lines` (list): List of detected lines.
   - **Returns:**
     - `filtered_lines` (list): Filtered lines based on length.

6. **`compute_intersections(lines)`**
   - Computes intersections between lines.
   - **Parameters:**
     - `lines` (list): List of lines.
   - **Returns:**
     - `intersections` (list): List of intersection points.

7. **`overlay_lines_and_corners(frame, lines, corners)`**
   - Overlays detected lines and corners on the frame.
   - **Parameters:**
     - `frame` (np.array): The frame to be modified.
     - `lines` (list): List of lines.
     - `corners` (list): List of corners.
   - **Returns:**
     - `frame` (np.array): Modified frame with overlays.

### Image Stitching Functions

1. **`extract_features(image)`**
   - Extracts features using SIFT.
   - **Parameters:**
     - `image` (np.array): The image for feature extraction.
   - **Returns:**
     - `key_points` (list): List of key points detected.
     - `d` (np.array): Descriptor matrix.

2. **`feature_matching(d1, d2)`**
   - Matches features between two images using FLANN-based matcher.
   - **Parameters:**
     - `d1`, `d2` (np.array): Descriptor matrices of two images.
   - **Returns:**
     - `perfect_matches` (list): List of matched features.

3. **`combine_images(key_points1, key_points2, matches, image1, image2)`**
   - Stitches two images together based on matched features.
   - **Parameters:**
     - `key_points1`, `key_points2` (list): Key points of the images.
     - `matches` (list): Matched features.
     - `image1`, `image2` (np.array): The images to be combined.
   - **Returns:**
     - `result` (np.array): The combined panoramic image.

## Output

- The output includes a processed video with overlays of detected lines and corners.
- A stitched panoramic image created from the provided set of images.

## Justification for SIFT

**SIFT (Scale-Invariant Feature Transform)** is used because it is robust to variations in scale, rotation, and illumination. This makes it suitable for matching corresponding points in overlapping images, especially when creating panoramas. SIFT features are distinctive and invariant to changes, ensuring high repeatability across multiple views of the same scene.

## Panoramic Mosaicing Explanation

Panoramic mosaicing, or panorama stitching, works better when the camera rotates around its center because:

1. **Minimization of Parallax Errors:** Parallax is minimized when the camera rotates around its center, preserving the scene geometry.
2. **Preservation of Scene Geometry:** This ensures accurate alignment and blending of images.
3. **Simplification of Image Registration:** Rotation-based registration is computationally simpler and more robust.
4. **Reduction of Stitching Artifacts:** Minimizes artifacts like ghosting or misalignment during blending.

## References

- [OpenCV Documentation](https://docs.opencv.org/)
- [NumPy Documentation](https://numpy.org/doc/)
- [Matplotlib Documentation](https://matplotlib.org/stable/contents.html)

---
