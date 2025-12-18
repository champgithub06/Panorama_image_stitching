# 🌄 Panorama Image Stitching - Computer Vision Project

This project builds a **complete panorama image stitching pipeline from scratch**, including feature matching, bundle adjustment, spherical warping, seam optimization, and advanced blending to generate **high-quality panoramic images**.

Just like how a phone camera creates a panorama, this project takes multiple overlapping images of the same scene and stitches them smoothly into a single wide image.

> ⚠️ **Important**
>
> This project uses **classical Computer Vision algorithms**  
> ❌ No Deep Learning  
> ❌ No OpenCV automatic stitcher

---

## 🧩 What is Panorama Image Stitching?

**Panorama stitching means:**

- You take multiple images of the same scene
- Each image overlaps slightly with the next
- The system finds common points between images
- Images are aligned geometrically
- All images are blended into **one large seamless image**

---

## 🧠 How This Project Works

### 1️⃣ Input Images
- All images are stored inside the `images/` folder
- Images are:
  - Taken from slightly different angles
  - Have overlapping regions

---

### 2️⃣ Feature Detection & Matching (`features.py`)
The first step is to find common points between images.

**What happens:**
- Important points (corners, edges) are detected using **SIFT / MSOP**
- Features are matched between image pairs
- Incorrect matches are removed using **Lowe’s Ratio Test**

👉 This step identifies which part of one image corresponds to another image.

---

### 3️⃣ Homography Estimation
Once matching points are found:

- A **homography matrix** is computed using **RANSAC**
- This matrix describes how one image should be transformed to align with another

👉 This step performs **geometric alignment** of images.

---

### 4️⃣ Bundle Adjustment (`Bundle_Adj.py`)
This is the **most important and advanced** part of the pipeline.

**Problem:**
- Small alignment errors accumulate when stitching many images

**Solution:**
- Bundle Adjustment optimizes **all camera parameters together**
- Uses **Levenberg–Marquardt optimization**
- Minimizes **reprojection error** between matched points

**Optimized parameters:**
- Camera rotation
- Camera focal length
- Camera center (principal point)

👉 This makes the panorama **accurate, stable, and distortion-free**.

---

### 5️⃣ Spherical Projection (`main.py`)
**Why is this needed?**
- Flat image stitching causes distortion for wide panoramas

**What happens:**
- Images are projected onto a **virtual sphere**
- Perspective distortion is reduced

👉 The panorama looks more **natural and smooth**.

---

### 6️⃣ Image Warping (`blend.py`)
Each image is:
- Warped into spherical coordinates
- Mapped to the correct position in panorama space
- Filled with transparent pixels where data is missing

---

### 7️⃣ Seam Finding (Graph Cut)
When images overlap:
- The system must decide **where to cut and join images**

This project uses:
- **Graph Cut algorithm**
- Finds seams where pixel differences are minimal

👉 This avoids visible stitching artifacts.

---

### 8️⃣ Image Blending
To hide seams and brightness differences, multiple blending techniques are used:

- **Alpha Blending** – Simple weighted averaging
- **Laplacian Pyramid Blending** – Multi-scale smooth blending
- **Poisson Blending** – Gradient-based seamless blending

👉 Final output looks like **one single photograph**.


## ▶️ How to Run the Project

Run the application by passing the path to the images folder and optional arguments.

### Basic Usage
```bash
python main.py images
```


---

⭐ If this project helped you understand panorama stitching, please **star the repository**!

---
