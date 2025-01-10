# aruco-markers
### README: Aruco Markers in Python

#### **Overview**
This repository demonstrates how to generate, detect, and manipulate **Aruco markers** using **OpenCV** in Python. Aruco markers are square-shaped markers that can be used for **augmented reality (AR)**, **robotics**, and **computer vision** tasks, especially for camera calibration and localization. Each marker has a unique ID encoded within the matrix of the marker, making it easily detectable.

In this repository, we cover how to:
1. Generate Aruco markers.
2. Detect Aruco markers from an image (including partial damage).
3. Customize detection parameters.
4. Display and manipulate Aruco markers.

#### **Contents**
- `generate_aruco_marker.py`: A Python script to generate Aruco markers.
- `detect_aruco_marker.py`: A Python script to detect Aruco markers from an image.
- `README.md`: Documentation for the repository.
- `images/`: Folder containing example images for testing.

#### **Prerequisites**
Make sure you have the following dependencies installed:
- **Python 3.x**
- **OpenCV** with the Aruco module (opencv-contrib-python)

You can install OpenCV with the contrib modules (which include Aruco) via pip:

```bash
pip install opencv-contrib-python
```

#### **How to Generate Aruco Markers**
To generate Aruco markers, we use the `cv2.aruco.drawMarker()` function from OpenCV. The script `generate_aruco_marker.py` can be used to generate markers with a specific ID and size.

##### **Usage**:
```bash
python generate_aruco_marker.py
```

This will generate a **200x200 pixel** Aruco marker with an ID of 69. The marker is saved as `aruco_marker_69.png`.

##### **Customizing the Generation**:
- To change the marker size, modify the `marker_size` parameter.
- To generate a marker with a different ID, change the `marker_id` parameter.
- To generate multiple markers, use the provided loop for generating a batch of markers with a range of IDs.

---

#### **How to Detect Aruco Markers**
The `detect_aruco_marker.py` script allows you to detect Aruco markers in an image. The marker detection can be customized for detecting damaged or noisy markers by adjusting detection parameters.

##### **Usage**:
```bash
python detect_aruco_marker.py
```

This will detect Aruco markers in the provided image. If the marker is detected successfully, the script will print the detected marker IDs and show the image with the marker highlighted.

##### **Handling Partial Damage**:
If the marker is damaged (i.e., part of the marker is missing or faded), you can adjust the detection parameters. The parameters that can be tuned are:
- `adaptiveThreshWinSizeMin`: Minimum window size for adaptive thresholding.
- `adaptiveThreshWinSizeMax`: Maximum window size for adaptive thresholding.
- `minMarkerPerimeterRate`: The minimum perimeter rate of the marker for detection.
- `perspectiveRemoveIgnoredMarginPerCell`: Helps with ignoring small damaged parts of the marker.

For example, to increase the robustness to damaged markers, you can set the following parameters in the detection function:
```python
parameters = cv2.aruco.DetectorParameters_create()
parameters.adaptiveThreshWinSizeMin = 3
parameters.adaptiveThreshWinSizeMax = 23
parameters.minMarkerPerimeterRate = 0.03
parameters.perspectiveRemoveIgnoredMarginPerCell = 0.15
```

---

#### **Example Code Snippets**

##### **Generate Aruco Marker**
```python
import cv2

# Define Aruco dictionary and marker ID
dictionary = cv2.aruco.getPredefinedDictionary(cv2.aruco.DICT_6X6_250)
marker_id = 69  # Can be changed
marker_size = 200  # Size in pixels

# Generate and save the marker
marker_image = cv2.aruco.drawMarker(dictionary, marker_id, marker_size)
cv2.imwrite(f"aruco_marker_{marker_id}.png", marker_image)
print(f"Marker {marker_id} saved.")
```

##### **Detect Aruco Marker**
```python
import cv2

# Load the image containing the Aruco marker
image = cv2.imread("path_to_image_with_marker.jpg")

# Define Aruco dictionary
dictionary = cv2.aruco.getPredefinedDictionary(cv2.aruco.DICT_6X6_250)

# Define detector parameters for more robust detection
parameters = cv2.aruco.DetectorParameters_create()
parameters.adaptiveThreshWinSizeMin = 3
parameters.adaptiveThreshWinSizeMax = 23
parameters.minMarkerPerimeterRate = 0.03
parameters.perspectiveRemoveIgnoredMarginPerCell = 0.15

# Detect markers
corners, ids, rejectedImgPoints = cv2.aruco.detectMarkers(image, dictionary, parameters=parameters)

# Draw detected markers
if len(corners) > 0:
    image_with_markers = cv2.aruco.drawDetectedMarkers(image, corners, ids)
    cv2.imshow("Detected Markers", image_with_markers)
    cv2.waitKey(0)
    cv2.destroyAllWindows()
else:
    print("No markers detected.")
```

---

#### **Common Issues and Solutions**

1. **Marker Not Detected**:
   - Ensure that the marker is not too far from the camera or too small.
   - Make sure the marker is not heavily occluded or distorted.
   - Try adjusting detection parameters to improve marker detection (see "Handling Partial Damage" section).

2. **Performance Issues**:
   - For faster detection, use smaller images or downscale the image before detection.
   - Adjust the thresholding parameters for better robustness in noisy environments.

3. **Multiple Markers Detection**:
   - If multiple markers need to be detected, the detection function will return multiple marker IDs. Make sure the markers do not overlap too much, as it could make detection more difficult.

---

#### **Additional Resources**

- **OpenCV Documentation**:
  - [Aruco Module Documentation](https://docs.opencv.org/4.x/d5/dae/tutorial_aruco_detection.html)
  - [OpenCV Python Documentation](https://docs.opencv.org/master/d6/d00/tutorial_py_root.html)

- **Aruco Markers**:
  - You can find a list of predefined Aruco marker dictionaries and their specifications in the OpenCV documentation, such as `DICT_4X4_50`, `DICT_5X5_100`, `DICT_6X6_250`, and others.

---

#### **Contributing**

If you'd like to contribute to this project or improve the detection process, feel free to fork this repository, make changes, and create a pull request. Contributions are welcome!

---


---

### **Conclusion**
This project provides a basic guide to generating and detecting Aruco markers using OpenCV. You can adjust the parameters for robust detection in challenging conditions, including partial damage to the marker. The Aruco marker system is widely used in computer vision applications and can be customized for a variety of tasks.
