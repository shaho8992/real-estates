import cv2
import numpy as np
import matplotlib.pyplot as plt

# Load the image
img = cv2.imread("kurd.jpg")  # Replace with the correct image file path
if img is None:
    print("Error: Image not found! Please check the file path.")
    exit()

# Convert the image to RGB and grayscale
imagergb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
img_gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

# Apply Gaussian blur
img_blur = cv2.GaussianBlur(img_gray, (5, 5), 0)

# Perform edge detection using the Canny method
edges = cv2.Canny(img_blur, 100, 200)

# Resize the edge-detected image
resized = cv2.resize(edges, (300, 300))

# Apply rotation
height, width = resized.shape[:2]
center = (width // 2, height // 2)
rotation_matrix = cv2.getRotationMatrix2D(center, 45, 1)
rotated = cv2.warpAffine(resized, rotation_matrix, (width, height))

# Apply contrast stretching
def contrast_stretching(image, in_range=(50, 200), out_range=(0, 255)):
    in_min, in_max = in_range
    out_min, out_max = out_range
    stretched = ((image - in_min) * ((out_max - out_min) / (in_max - in_min)) + out_min)
    return np.clip(stretched, out_min, out_max).astype(np.uint8)

contrast_image = contrast_stretching(img_gray)

# Apply histogram equalization
hist_eq_image = cv2.equalizeHist(img_gray)

# Apply logarithmic transformation
def logarithmic_transformation(image):
    c = 255 / (np.log(1 + np.max(image)))  # Scale factor
    log_image = c * np.log(1 + image.astype(np.float32))
    return np.clip(log_image, 0, 255).astype(np.uint8)

log_image = logarithmic_transformation(img_gray)

# Apply gamma correction
def gamma_correction(image, gamma):
    normalized_image = image / 255.0
    gamma_corrected = (255 * (normalized_image ** gamma)).astype(np.uint8)
    return gamma_corrected

gamma_corrected = gamma_correction(img_gray, gamma=0.5)

# Display results
plt.figure(figsize=(15, 12))

plt.subplot(3, 3, 1)
plt.imshow(imagergb)
plt.title("Original Image (RGB)")
plt.axis('off')

plt.subplot(3, 3, 2)
plt.imshow(img_gray, cmap='gray')
plt.title("Grayscale Image")
plt.axis('off')

plt.subplot(3, 3, 3)
plt.imshow(edges, cmap='gray')
plt.title("Edge Detection (Canny)")
plt.axis('off')

plt.subplot(3, 3, 4)
plt.imshow(resized, cmap='gray')
plt.title("Resized Edge Image")
plt.axis('off')

plt.subplot(3, 3, 5)
plt.imshow(rotated, cmap='gray')
plt.title("Rotated Edge Image")
plt.axis('off')

plt.subplot(3, 3, 6)
plt.imshow(contrast_image, cmap='gray')
plt.title("Contrast Stretching")
plt.axis('off')

plt.subplot(3, 3, 7)
plt.imshow(log_image, cmap='gray')
plt.title("Logarithmic Transformation")
plt.axis('off')

plt.subplot(3, 3, 8)
plt.imshow(hist_eq_image, cmap='gray')
plt.title("Histogram Equalization")
plt.axis('off')

plt.tight_layout()
plt.show()

# Plot histograms for grayscale and histogram-equalized images
plt.figure(figsize=(12, 6))

# Original histogram
plt.subplot(1, 2, 1)
plt.hist(img_gray.ravel(), bins=256, range=[0, 256], color='blue', alpha=0.7)
plt.title("Original Grayscale Histogram")
plt.xlabel("Pixel Intensity")
plt.ylabel("Frequency")
plt.grid()

# Equalized histogram
plt.subplot(1, 2, 2)
plt.hist(hist_eq_image.ravel(), bins=256, range=[0, 256], color='green', alpha=0.7)
plt.title("Histogram Equalized")
plt.xlabel("Pixel Intensity")
plt.ylabel("Frequency")
plt.grid()

plt.tight_layout()
plt.show()


# Observations
print("Summary of Image Processing Techniques:")
print("1. Grayscale conversion simplifies the image to intensity values.")
print("2. Gaussian blur reduces noise and smooths the image.")
print("3. Canny edge detection highlights prominent edges.")
print("4. Resizing changes the image dimensions, useful for standardization.")
print("5. Rotation transforms the image for orientation analysis.")
print("6. Contrast stretching enhances the intensity range for better visibility.")
print("7. Logarithmic transformation emphasizes low-intensity details.")
print("8. Gamma correction brightens or darkens the image based on the gamma value.")
print("9. Histogram equalization improves contrast by redistributing intensity values.")
print("10. The histogram for equalized images shows a uniform distribution, indicating enhanced contrast.")
