# Sturdy-Octo-Disco-Adding-Sunglasses-for-a-Cool-New-Look

Sturdy Octo Disco is a fun project that adds sunglasses to photos using image processing.

Welcome to Sturdy Octo Disco, a fun and creative project designed to overlay sunglasses on individual passport photos! This repository demonstrates how to use image processing techniques to create a playful transformation, making ordinary photos look extraordinary. Whether you're a beginner exploring computer vision or just looking for a quirky project to try, this is for you!

## Features:
- Detects the face in an image.
- Places a stylish sunglass overlay perfectly on the face.
- Works seamlessly with individual passport-size photos.
- Customizable for different sunglasses styles or photo types.

## Technologies Used:
- Python
- OpenCV for image processing
- Numpy for array manipulations

## How to Use:
1. Clone this repository.
2. Add your passport-sized photo to the `images` folder.
3. Run the script to see your "cool" transformation!

## Applications:
- Learning basic image processing techniques.
- Adding flair to your photos for fun.
- Practicing computer vision workflows.
## Program & Output
~~~
# Import libraries
import cv2
import numpy as np
import matplotlib.pyplot as plt

# Load the Face Image
faceImage = cv2.imread('me.png')
plt.imshow(faceImage[:,:,::-1]);plt.title("Face")

faceImage.shape

scale_percent = 50
new_width = int(faceImage.shape[1] * scale_percent / 100)
new_height = int(faceImage.shape[0] * scale_percent / 100)
dim = (new_width, new_height)

resized_faceImage = cv2.resize(faceImage, dim, interpolation=cv2.INTER_AREA)
plt.imshow(resized_faceImage[..., ::-1])
plt.axis('on')
plt.show()

faceImage.shape

# Load the Sunglass image with Alpha channel
# (http://pluspng.com/sunglass-png-1104.html)
glassPNG = cv2.imread('images (7).jpeg',-1)
plt.imshow(glassPNG[:,:,::-1]);plt.title("glassPNG")

# Resize the image to fit over the eye region
glassPNG = cv2.resize(glassPNG,(190,50))
print("image Dimension ={}".format(glassPNG.shape))

# Separate the Color and alpha channels
glassBGR = glassPNG[:,:,0:2]
glassMask1 = glassPNG[:,:,2]

# Display the images for clarity
plt.figure(figsize=[15,15])
plt.subplot(121);plt.imshow(glassBGR[:,:,::-1]);plt.title('Sunglass Color channels');
plt.subplot(122);plt.imshow(glassMask1,cmap='gray');plt.title('Sunglass Alpha channel');

# Copy your resized face image
faceWithGlasses = resized_faceImage.copy()

# Load sunglasses with alpha channel (4th channel is transparency)
glassPNG = cv2.imread('images (7).jpeg', -1)

# Resize sunglasses to fit your face width
glass_resized = cv2.resize(glassPNG, (200, 70))  # width=200, height=70

# Split color and alpha channels safely
if glass_resized.shape[2] == 4:
    glass_rgb = glass_resized[:, :, :3]
    alpha_mask = glass_resized[:, :, 3]
else:
    # if no alpha channel, create a mask from non-white pixels
    glass_rgb = glass_resized
    gray = cv2.cvtColor(glass_rgb, cv2.COLOR_BGR2GRAY)
    _, alpha_mask = cv2.threshold(gray, 250, 255, cv2.THRESH_BINARY_INV)

# Convert mask to boolean region
mask = alpha_mask > 0

# Make sure the sunglasses are pure black (hide eyes)
glass_rgb[mask] = [0, 0, 0]   # set visible sunglass pixels to solid black

# Define region (adjust if needed)
y1, y2 = 210, 280   # vertical position
x1, x2 = 190, 390   # horizontal position

# Extract face region
roi = faceWithGlasses[y1:y2, x1:x2]

# Overlay sunglasses (replace pixels where mask is true)
roi[mask] = glass_rgb[mask]

# Put back into main image
faceWithGlasses[y1:y2, x1:x2] = roi

# Display
plt.imshow(faceWithGlasses[..., ::-1])
plt.axis('off')
plt.show()

~~~

<img width="509" height="532" alt="Screenshot 2026-02-03 164051" src="https://github.com/user-attachments/assets/8a8e3ae0-ca9d-48c5-bec3-4685a7428805" />

<img width="160" height="46" alt="Screenshot 2026-02-03 164056" src="https://github.com/user-attachments/assets/b0f55e9c-b43d-416b-aa41-5c2eae24fdfa" />

<img width="725" height="388" alt="Screenshot 2026-02-03 164105" src="https://github.com/user-attachments/assets/893219d2-a503-4572-b95f-0ce018dc65e0" />

<img width="270" height="49" alt="Screenshot 2026-02-03 164112" src="https://github.com/user-attachments/assets/b947c197-f598-4335-9c08-cf6b90873b04" />

<img width="1382" height="223" alt="Screenshot 2026-02-03 164124" src="https://github.com/user-attachments/assets/b493116d-c358-4e72-af9d-00f80882fda5" />

<img width="537" height="479" alt="Screenshot 2026-02-03 164132" src="https://github.com/user-attachments/assets/d56b8159-a8b5-41f2-812e-e1a1f9cbcf91" />

Feel free to fork, contribute, or customize this project for your creative needs!
