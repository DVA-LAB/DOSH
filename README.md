ENGLISH | [KOREAN](https://github.com/DVA-LAB/DVA_LAB-data/blob/main/README-KO.md)

# Jeju Dolphin & Ship Detection Dataset

## Overview
This dataset was created to detect Indo-Pacific bottlenose dolphins and ships in the waters around Jeju, using aerial and marine video footage. 
Key features include data collected under various environmental conditions (weather, time of day, etc.), with annotations that consider changes in object size and shape.

## Fellow Organization Information
This dataset was sourced by MARC, and the labeling was carried out by members of DVA LAB's 2nd cohort.

- **Organization**: MARC (Marine Animal Research and Conservation)  
- **Contact Person**: Miyeon Kim, Deputy Director  
- **Email**: marckorea718@gmail.com  

## License
- **License**: CC BY-NC-SA (Attribution-NonCommercial-ShareAlike)
- **Usage Scope**: May be used for non-commercial research and educational purposes, with proper attribution required.
- **Derivative Works Guidelines**: Any derivative works must be distributed under the same license, with clear documentation of modifications.

## Dataset Specification

### Data Structure
This dataset consists of image data and corresponding annotation files, stored in the following directory structure:

- **ROOT Directory**: `private_files_for_service/AI/object_detection`
- **Dataset Path**: `ROOT/dataset_2024/unpatched/`
- **Image Directories**:
  - `train_1 ~ train_7`
  - `val_1 ~ val_2`
  - `test_1`
- **Annotation Directories**:
  - Contains 10 bbox annotation files corresponding to the image splits.
  - Annotation format: **CVAT 1.1 (XML format)**

### Data Statistics
- **Total Dataset Size**: Approximately 50GB
- **Number of Images**: 1,085
- **Resolutions**: Mostly `3840×2160`, some `1920×1080`
- **Label Types**: Dolphin, Ship, Jetski
- **Collection Period**: January 2023 – December 2024

### Object-wise Bbox Statistics

| Class   | bbox Count | bbox Per Image | Mean Size | Median Size | Min Size | Max Size |
|---------|------------|----------------|------------|------------|-----------|-----------|
| Dolphin | 834        | 0.8            | 1,662      | 1,018      | 66        | 22,302    |
| Jetski  | 85         | 0.1            | 2,339      | 1,736      | 66        | 8,875     |
| Ship    | 952        | 0.9            | 40,276     | 26,992     | 90        | 501,071   |

### Bbox Size Distribution
- **Jetski**: Objects are evenly distributed across various sizes.
- **Dolphin, Ship**: Most objects are small, with a few extreme outliers.

## Preprocessing Information

### Data Cleaning Process
- **Duplicate Removal**: Filtering out repetitive frames.
- **Image Quality Enhancement**: Brightness, contrast, and blurriness adjustments.
- **Noise Reduction**: Eliminating motion blur and irrelevant data.

### Labeling Method
- **Annotations were created using CVAT 1.1**
- **BBox Annotation**:
  - Initially, Axis-Aligned Bounding Boxes (AABB) were used.
  - Rotated bounding boxes (OBB) were later added for additional model experiments.

### Quality Validation
- **Multi-stage verification process**:
  - **Stage 1**: Initial labeling following predefined guidelines.
  - **Stage 2**: Consistency check and review by a second annotator.
  - **Stage 3**: Model testing to identify misdetections and refine labels.

## Access Information

### Data Download
- **Download Link**: (To be provided later)
- **Access Request**: taylor.7@kakaoimpact.org
- **Data Format**:
  - Image data (`.png`)
  - Annotation data (`.xml` - CVAT 1.1)

## Usage Example

### Sample Code to Load Data
```python
import os
import xml.etree.ElementTree as ET
import cv2

# Dataset Path
ROOT = "private_files_for_service/AI/object_detection"
DATASET_PATH = os.path.join(ROOT, "dataset_2024/unpatched/images/train_1")

# Load Annotation File
ANNOTATION_PATH = os.path.join(ROOT, "dataset_2024/unpatched/annotations/train_1.xml")

# Parse XML
tree = ET.parse(ANNOTATION_PATH)
root = tree.getroot()

# Display Image with Bboxes
for image in root.findall('image'):
    image_name = image.get('name')
    img_path = os.path.join(DATASET_PATH, image_name)
    img = cv2.imread(img_path)
    
    for box in image.findall('box'):
        label = box.get('label')
        xtl = int(float(box.get('xtl')))
        ytl = int(float(box.get('ytl')))
        xbr = int(float(box.get('xbr')))
        ybr = int(float(box.get('ybr')))
        
        # Draw Bounding Box
        cv2.rectangle(img, (xtl, ytl), (xbr, ybr), (0, 255, 0), 2)
        cv2.putText(img, label, (xtl, ytl-10), cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0, 255, 0), 2)

    cv2.imshow("Image", img)
    cv2.waitKey(0)

cv2.destroyAllWindows()
```

## Notes

### Limitations
- Commercial use is strictly prohibited.
- Unauthorized redistribution of dataset images is not allowed.

### Known Issues
- **Dark Images**: Dolphins may be difficult to detect when submerged.
- **Water Spray**: Ship trails may obscure objects.
- **Light Reflection**: Surface reflections can obscure object boundaries.

### Recommendations
- **Preprocessing Adjustments**: Enhancing brightness and contrast can improve object detection performance.
- **Data Augmentation**: Applying techniques to simulate varied lighting conditions is recommended.

## Update History

| Version | Date       | Description                   | Author   |
|---------|-----------|------------------------------|---------|
| 0.1     | 2024-02-18 | Initial dataset README.md setup | Seowoo Han |

## Contact Information
- **Slack Channel**: `#99-lab-qna`
- **Contact Person**: Minseok Kim (Kakao Impact)
- **Email**: taylor.7@kakaoimpact.org
