🌄 Panorama Image Stitching from Scratch

This project builds a complete image stitching pipeline from scratch, including feature matching, bundle adjustment, spherical warping, seam optimization, and advanced blending, to generate high-quality panoramic images.
Just like how a phone camera makes a panorama, this project takes many images of the same scene and stitches them together smoothly.

👉 Important:
This is done using classical Computer Vision algorithms, not deep learning and not OpenCV’s automatic stitcher

🧩 What is Panorama Image Stitching?

----> Panorama stitching means:
----> You take multiple images
----> Each image overlaps a little with the next one
----> The system finds common points
----> It aligns the images
----> It blends them smoothly into one large image

🧠 How This Project Works
1️⃣ Input Images
----> All images are stored inside the images/ folder.
----> These images: Are taken from slightly different angles , Have overlapping regions

2️⃣ Feature Detection & Matching (features.py)
The first step is to find common points between images.
What happens:
----> Important points (corners, edges) are detected using SIFT / MSOP
----> These points are matched between image pairs
----> Wrong matches are removed using Lowe’s ratio test
👉 This tells the system which part of one image matches another image

3️⃣ Homography Estimation
Once matching points are found:
----> A homography matrix is computed using RANSAC
----> This matrix describes how one image should be transformed to align with another
👉 This step aligns images geometrically

4️⃣ Bundle Adjustment (Bundle_Adj.py)
This is the most important and advanced part.
Problem:
---->  Small alignment errors accumulate when stitching many images
Solution:
----> Bundle Adjustment optimizes all camera parameters together
----> Uses Levenberg–Marquardt optimization
----> Minimizes reprojection error between matched points
What is optimized:
----> Camera rotation
----> Camera focal length
----> Camera center (principal point)
👉 This makes the panorama accurate and stable

5️⃣ Spherical Projection (main.py)
Why needed?
----> Flat images cause distortion when stitching wide scenes
What happens:
----> Images are projected onto a virtual sphere
----> This reduces perspective distortion
👉 This step makes the panorama look natural and smooth

6️⃣ Image Warping (blend.py)
Each image is:
----> Warped into spherical coordinates
----> Mapped to its correct position in panorama space
----> Transparent pixels are added where data is missing

7️⃣ Seam Finding (Graph Cut)
When images overlap:
----> We must decide where to cut and join them
This project uses:
----> Graph Cut algorithm
----> Finds the seam where the difference between images is minimum
👉 This avoids visible stitching lines

8️⃣ Image Blending
To hide seams and brightness differences, multiple blending techniques are used:
----> Alpha Blending – simple weighted average
----> Laplacian Pyramid Blending – multi-scale smooth blending
----> Poisson Blending – gradient-based seamless blending
👉 Final output looks like one single photo

⭐ If this project helped you understand panorama stitching, please star the repository.

▶️ How to Run the Project
How to Run
Run the application by passing the path to the images folder and optional arguments.

Basic Usage
python main.py images

⚙️ Command-Line Options
🔹 Image Downsampling
-s, --shrink SHRINK

Downsamples images by a factor of SHRINK to speed up processing.
Very large values may reduce the number of detected features and cause stitching to fail.


python main.py images -s 2

🔹 Bundle Adjustment
--ba {none,incr,last}

Controls how camera parameters are optimized:
none – Skip bundle adjustment
incr – Apply bundle adjustment incrementally (default)
last – Apply bundle adjustment after all images are added

🔹 Exposure Compensation
-e, --equalize

Compensates exposure differences between images.
Disabled by default as it may introduce color artifacts.

🔹 Crop Final Panorama
-c, --crop

Crops the largest valid rectangle from the final stitched panorama.

🔹 Blending Method
-b, --blend {none,linear,multiband}

Specifies how overlapping regions are blended:
none – Simple pasting
linear – Linear blending
multiband – Multi-band blending (default, best quality)

