# Image File Processing

<p align="center">
  <img src="Screenshots/01_original.png" alt="Image File Processing Interface" width="900">
</p>

<p align="center">
  <b>Modern C/C++ BMP image-processing desktop application built with EasyX.</b>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Language-C%2FC%2B%2B-00599C?style=for-the-badge&logo=cplusplus&logoColor=white" alt="C/C++">
  <img src="https://img.shields.io/badge/GUI-EasyX-2563EB?style=for-the-badge" alt="EasyX">
  <img src="https://img.shields.io/badge/IDE-Visual%20Studio-5C2D91?style=for-the-badge&logo=visualstudio&logoColor=white" alt="Visual Studio">
  <img src="https://img.shields.io/badge/Platform-Windows-0078D6?style=for-the-badge&logo=windows&logoColor=white" alt="Windows">
  <img src="https://img.shields.io/badge/Image%20Format-24--bit%20BMP-16A34A?style=for-the-badge" alt="BMP">
</p>

---

## Overview

**Image File Processing** is a desktop image-processing application developed in **C/C++** using the **EasyX graphics library**.

The program reads a **24-bit uncompressed BMP image**, displays it through a modern dark graphical interface, and applies multiple image-processing operations such as grayscale conversion, Sobel edge detection, sharpening, salt-and-pepper noise generation, and median filtering.

This project demonstrates practical skills in:

* Binary BMP file processing
* Pixel-level image manipulation
* C/C++ memory management
* Convolution-based image algorithms
* Modular software design
* EasyX graphical interface development
* Debugging of low-level image format issues

---

## Key Features

| Feature               | Description                                               |
| --------------------- | --------------------------------------------------------- |
| BMP Loader            | Reads and validates 24-bit uncompressed BMP files         |
| Image Preview         | Displays the image inside a scaled preview area           |
| Grayscale Conversion  | Converts BGR image data into grayscale intensity values   |
| Sobel Edge Detection  | Extracts major image contours using gradient operators    |
| Image Sharpening      | Enhances image details using a 3×3 convolution kernel     |
| Salt-and-Pepper Noise | Adds random black and white noise to simulate corruption  |
| Median Filtering      | Reduces salt-and-pepper noise using a 3×3 median filter   |
| Modern GUI            | Dark photo-editor style interface built with EasyX        |
| Status Panel          | Displays operation feedback directly inside the interface |
| BMP Export            | Saves processed results as BMP image files                |

---

## Application Preview

<p align="center">
  <img src="Screenshots/01_original.png" alt="Original BMP View" width="900">
</p>

<p align="center">
  <i>Modern EasyX interface with image preview, operation panel, and status display.</i>
</p>

---

## Processing Results

<table>
  <tr>
    <td align="center">
      <img src="Screenshots/01_original.png" width="420"><br>
      <b>Original BMP</b>
    </td>
    <td align="center">
      <img src="Screenshots/02_grayscale.png" width="420"><br>
      <b>Grayscale Conversion</b>
    </td>
  </tr>
  <tr>
    <td align="center">
      <img src="Screenshots/03_edge_detection.png" width="420"><br>
      <b>Sobel Edge Detection</b>
    </td>
    <td align="center">
      <img src="Screenshots/04_sharpen.png" width="420"><br>
      <b>Image Sharpening</b>
    </td>
  </tr>
  <tr>
    <td align="center">
      <img src="Screenshots/05_noise.png" width="420"><br>
      <b>Salt-and-Pepper Noise</b>
    </td>
    <td align="center">
      <img src="Screenshots/06_median_filter.png" width="420"><br>
      <b>Median Filtering</b>
    </td>
  </tr>
</table>

---

## Processing Pipeline

```text
24-bit BMP Input
      │
      ▼
BMP Validation
      │
      ├── Check BMP signature
      ├── Check bit depth
      ├── Check compression type
      └── Calculate row stride
      │
      ▼
Load Pixel Data
      │
      ├── Display Original Image
      ├── Convert to Grayscale
      ├── Apply Sobel Edge Detection
      ├── Apply Image Sharpening
      ├── Add Salt-and-Pepper Noise
      └── Apply Median Filtering
      │
      ▼
Save Processed BMP Outputs
```

---

## System Design

```text
ImageFileProcessing
│
├── GUI Layer
│   ├── Draw application window
│   ├── Draw image preview area
│   ├── Draw control panel
│   ├── Draw operation buttons
│   └── Display status messages
│
├── BMP File Layer
│   ├── Read BMP header
│   ├── Validate BMP format
│   ├── Load pixel data
│   └── Save processed BMP files
│
├── Image Processing Layer
│   ├── Grayscale conversion
│   ├── Sobel edge detection
│   ├── Image sharpening
│   ├── Noise generation
│   └── Median filtering
│
└── Display Layer
    ├── Scale image preview
    ├── Render 24-bit color image
    └── Render 8-bit grayscale image
```

---

## Core Functions

| Function                  | Responsibility                                |
| ------------------------- | --------------------------------------------- |
| `calculate_stride()`      | Calculates BMP row size with 4-byte alignment |
| `read_bmp()`              | Reads and validates a 24-bit BMP image        |
| `save_bmp_8bit()`         | Saves grayscale results as 8-bit BMP files    |
| `save_bmp_24bit()`        | Saves color results as 24-bit BMP files       |
| `convert_24bit_to_gray()` | Converts BGR pixels to grayscale              |
| `edge_detection()`        | Performs Sobel edge detection                 |
| `image_sharpening()`      | Applies sharpening convolution                |
| `addSaltPepperNoise()`    | Adds random black and white noise             |
| `medianFiltering()`       | Removes noise using median filtering          |
| `showImage24Fit()`        | Displays scaled 24-bit images                 |
| `showImage8Fit()`         | Displays scaled 8-bit grayscale images        |
| `drawUI()`                | Renders the graphical user interface          |

---

## Algorithm Highlights

### BMP Row Alignment

BMP rows are aligned to 4-byte boundaries.
The program calculates the real row size using:

```cpp
int calculate_stride(int width, int bit_count)
{
    return ((width * bit_count + 31) / 32) * 4;
}
```

This prevents distorted output caused by ignoring BMP padding bytes.

---

### BGR Color Order

BMP stores color pixels in **BGR order**, not RGB order.

```text
Pixel memory order:
Blue → Green → Red
```

When displaying pixels in EasyX, the program converts the values correctly:

```cpp
RGB(R, G, B)
```

---

### Grayscale Conversion

The grayscale value is computed using the standard weighted formula:

```text
Gray = 0.299R + 0.587G + 0.114B
```

This produces visually balanced grayscale output because the human eye is more sensitive to green intensity.

---

### Sobel Edge Detection

Sobel edge detection uses two 3×3 kernels to calculate horizontal and vertical gradients.

```cpp
int sobel_x[3][3] = {
    {-1, 0, 1},
    {-2, 0, 2},
    {-1, 0, 1}
};

int sobel_y[3][3] = {
    {-1, -2, -1},
    { 0,  0,  0},
    { 1,  2,  1}
};
```

Gradient magnitude:

```text
G = sqrt(Gx² + Gy²)
```

Pixels with gradient magnitude above the threshold are marked as edges.

---

### Image Sharpening

The sharpening operation uses a 3×3 convolution kernel:

```cpp
int kernel[3][3] = {
    {-1, -1, -1},
    {-1,  9, -1},
    {-1, -1, -1}
};
```

This strengthens the center pixel and subtracts neighboring values, increasing local contrast and improving image detail.

---

### Median Filtering

Median filtering is used to reduce salt-and-pepper noise.

```text
1. Select a 3×3 neighborhood.
2. Collect the 9 pixel values.
3. Sort the values.
4. Replace the center pixel with the median value.
```

For color images, the B, G, and R channels are processed separately.

---

## Build and Run

### Requirements

* Windows
* Visual Studio
* EasyX graphics library
* 24-bit uncompressed BMP image

### Build Steps

1. Install Visual Studio.
2. Install and configure EasyX.
3. Open the project source files in Visual Studio.
4. Make sure the test BMP image exists.
5. Build the project.
6. Run the executable.
7. Use the GUI buttons to process the image.

During testing, the input image path was:

```text
C:\CP3\test.bmp
```

---

## Generated Outputs

| File          | Description                  |
| ------------- | ---------------------------- |
| `gray.bmp`    | Grayscale output             |
| `edge.bmp`    | Sobel edge detection output  |
| `sharpen.bmp` | Sharpened output             |
| `noise.bmp`   | Salt-and-pepper noise output |
| `median.bmp`  | Median filtering output      |

---

## Testing

| Test Case | Operation     | Expected Result                           | Status |
| --------- | ------------- | ----------------------------------------- | ------ |
| TC-01     | Read BMP      | Original image displayed correctly        | Passed |
| TC-02     | Gray Image    | Grayscale image generated and saved       | Passed |
| TC-03     | Edge Detect   | Edge contours extracted and saved         | Passed |
| TC-04     | Sharpen       | Image details enhanced and saved          | Passed |
| TC-05     | Add Noise     | Salt-and-pepper noise generated and saved | Passed |
| TC-06     | Median Filter | Noise reduced and result saved            | Passed |

---

## Technical Challenges

| Challenge                  | Cause                                              | Solution                                           |
| -------------------------- | -------------------------------------------------- | -------------------------------------------------- |
| BMP file failed to open    | Incorrect file path                                | Used a verified absolute path during testing       |
| Invalid BMP format         | File extension was changed without real conversion | Converted the image properly to BMP format         |
| Wrong color display        | BMP stores pixels as BGR                           | Converted display order to RGB                     |
| Row distortion             | BMP rows require 4-byte alignment                  | Implemented stride calculation                     |
| Image exceeded window size | Original image was larger than preview area        | Implemented image preview scaling                  |
| Basic initial interface    | Early version lacked visual polish                 | Redesigned the GUI into a dark photo-editor layout |

---

## Limitations

* Supports only 24-bit uncompressed BMP images.
* Input path is fixed in the current version.
* Algorithm parameters are not adjustable from the GUI.
* JPG and PNG formats are not supported.
* No side-by-side comparison mode yet.

---

## Future Improvements

* Add file selection dialog.
* Add adjustable Sobel threshold.
* Add adjustable noise ratio.
* Add support for JPG and PNG images.
* Add before/after comparison view.
* Add brightness and contrast adjustment.
* Improve performance for very large images.
* Add undo/reset functionality.

---

## Academic Report

The complete academic report is included in this repository:

```text
ImageFileProcessing_Report.md
```

It contains full documentation of the project objectives, system design, algorithms, implementation details, testing results, problems solved, and reflections.

---

## Author

**HANAN OSSAMA**
Artificial Intelligence Student
Harbin Institute of Technology
Student ID: **2025130323**

---

## Notes

This project was developed as part of the **Computing and Intelligent Programming** comprehensive practice project.
It is intended for academic learning, C/C++ programming practice, and image-processing system implementation.
