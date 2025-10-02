# Resize-Image-for-CNN-Input..
import cv2
import numpy as np
import matplotlib.pyplot as plt
from tkinter import Tk, filedialog

# Open file dialog
Tk().withdraw()  # hide the main Tkinter window
file_path = filedialog.askopenfilename(
    title="Select an image",
    filetypes=[("Image files", "*.jpg *.jpeg *.png *.bmp *.tiff *.webp")]
)

# Load image
img = cv2.imread(r"C:\Users\kumar\OneDrive\Desktop\All Notes\EVS\R.jpg")
if img is None:
    raise FileNotFoundError("No valid image selected or file not found.")

img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)

# 1. Basic Resizing
basic_resized = cv2.resize(img, (224, 224))

# 2. Aspect Ratio Preservation (Letterbox)
h, w, _ = img.shape
scale = min(224 / w, 224 / h)
new_w, new_h = int(w * scale), int(h * scale)
resized_img = cv2.resize(img, (new_w, new_h))
letterboxed = np.full((224, 224, 3), 128, dtype=np.uint8)  # gray padding
letterboxed[(224 - new_h) // 2:(224 - new_h) // 2 + new_h,
            (224 - new_w) // 2:(224 - new_w) // 2 + new_w] = resized_img

# 3. Center Crop
min_dim = min(h, w)
start_x = (w - min_dim) // 2
start_y = (h - min_dim) // 2
center_crop = img[start_y:start_y + min_dim, start_x:start_x + min_dim]
center_crop = cv2.resize(center_crop, (224, 224))

# Display results
titles = ["Original", "Basic Resize", "Letterbox", "Center Crop"]
images = [img, basic_resized, letterboxed, center_crop]

plt.figure(figsize=(10, 5))
for i in range(4):
    plt.subplot(1, 4, i + 1)
    plt.imshow(images[i])
    plt.title(titles[i])
    plt.axis("off")
plt.show()
<img width="986" height="579" alt="Screenshot 2025-09-29 135919" src="https://github.com/user-attachments/assets/7ddfc508-1280-4a88-a436-ba7c06cead85" />

