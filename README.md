ENGLISH | [KOREAN](https://github.com/DVA-LAB/DVA_LAB-data/blob/main/README-KO.md)

# DOSH dataset
**DO**lphin & **SH**ip Dataset for Object Detection

## 1. Overview
This dataset was created to detect Indo-Pacific bottlenose dolphins and ships in the waters around Jeju, using aerial and marine video footage. It includes diverse scenes considering three aspects. 

1. object movement
2. visual background (weather, time of day)
3. perspective of the camera

Additionally, to account for the free movement of objects from an aerial perspective, the dataset has annotations labeled with OBB (Oriented Bounding Box).

## 2. Fellow Organization Information
This dataset was sourced by MARC, and the labeling was carried out by members of DVA LAB's 2nd cohort.

- **Organization**: MARC (Marine Animal Research and Conservation)  
- **Contact Person**: Miyeon Kim, Deputy Director  
- **Email**: marckorea718@gmail.com  

## 3. License
- **License**: CC BY-NC-SA (Attribution-NonCommercial-ShareAlike)
- **Usage Scope**: May be used for non-commercial research and educational purposes, with proper attribution required.
- **Derivative Works Guidelines**: Any derivative works must be distributed under the same license, with clear documentation of modifications.

## 4. Dataset Specification

### 4.1. Data Structure
This dataset consists of image data and corresponding annotation files, stored in the following directory structure:

- **ROOT Directory**: `private_files_for_service/AI/DOSH`
- **Dataset Path**:
  - images: `ROOT/images/`
  - annotations: `ROOT/annotations/`
- **Image Sub-Directories**:
  - `train_1 ~ train_7`
  - `val_1 ~ val_2`
  - `test_1`
- **Annotation Sub-Directories**:
  - Contains 10 bbox annotation files corresponding to the image splits.
  - Annotation format: **[CVAT 1.1](https://docs.cvat.ai/docs/manual/advanced/xml_format/#version-11) (XML format)**

### 4.2. Data Statistics
The characteristics and basic statistics of the images in this dataset are as follows.
- **Total Dataset Size**: Approximately 50GB
- **Number of Images**: 1,085
- **Resolutions**: Mostly `3840×2160`, some `1920×1080`
- **Label Types**: Dolphin, Ship, Jetski
- **Collection Period**: January 2023 – December 2024

#### Object-wise Bbox Statistics

| Class   | bbox Count | bbox Per Image | Mean Size | Median Size | Min Size | Max Size |
|---------|------------|----------------|------------|------------|-----------|-----------|
| Dolphin | 834        | 0.8            | 1,662      | 1,018      | 66        | 22,302    |
| Jetski  | 85         | 0.1            | 2,339      | 1,736      | 66        | 8,875     |
| Ship    | 952        | 0.9            | 40,276     | 26,992     | 90        | 501,071   |

#### Bbox Size Distribution
- **Jetski**: Objects are evenly distributed across various sizes.
- **Dolphin, Ship**: Most objects are small, with a few extreme outliers.

## 5. Preprocessing Information

### 5.1. Data Cleaning Process
- **Duplicate Removal**: Filtering out repetitive frames.
- **Image Quality Enhancement**: Brightness, contrast, and blurriness adjustments.
- **Noise Reduction**: Eliminating motion blur and irrelevant data.

### 5.2. Labeling Method
- **Annotations were created using CVAT 1.1**
- **BBox Annotation**:  
  - OBB (Oriented Bounding Box) is defined by:  
    - xtl (top-left x), ytl (top-left y), xbr (bottom-right x), ybr (bottom-right y), rotation.
    - The rotation angle is measured counterclockwise in degrees, with 0 degrees aligned to the horizontal right direction.
  - In our experiments on this dataset, OBB-based models showed better detection performance than AABB(Axis-Aligned Bounding Box)-Based Models.

### 5.3. Quality Validation
- **Multi-stage verification process**:
  - **Stage 1**: Initial labeling following predefined guidelines.
  - **Stage 2**: Consistency check and review by a second annotator.
  - **Stage 3**: Model testing to identify misdetections and refine labels.

## 6. Access Information
You can request access as instructed to download the dataset.
### Data Download
- **Download Link**: [DOSH Download](https://drive.google.com/drive/u/0/folders/1paj06J-G-hm_dqtR2Bhi__6BEyvqzJTe)
- **Access Request**: taylor.7@kakaoimpact.org
- **Data Format**:
  - Image data (`.png`)
  - Annotation data (`.xml` - CVAT 1.1)

## 7. Usage Example

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
| 0.1     | 2025-02-18 | Initial dataset README.md setup | Seowoo Han |
| 0.2     | 2025-02-23 | Added an explanation of the labeling method  | Minseong Cho |

## Contact Information
- **Slack Channel**: `#99-lab-qna`
- **Contact Person**: Minseok Kim (Kakao Impact)
- **Email**: taylor.7@kakaoimpact.org
