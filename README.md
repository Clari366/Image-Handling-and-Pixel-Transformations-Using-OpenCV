# Image-Handling-and-Pixel-Transformations-Using-OpenCV 

## AIM:
Write a Python program using OpenCV that performs the following tasks:

1) Read and Display an Image.  
2) Adjust the brightness of an image.  
3) Modify the image contrast.  
4) Generate a third image using bitwise operations.

## Software Required:
- Anaconda - Python 3.7
- Jupyter Notebook (for interactive development and execution)

## Algorithm:
### Step 1:
Load an image from your local directory and display it.

### Step 2:
Create a matrix of ones (with data type float64) to adjust brightness.

### Step 3:
Create brighter and darker images by adding and subtracting the matrix from the original image.  
Display the original, brighter, and darker images.

### Step 4:
Modify the image contrast by creating two higher contrast images using scaling factors of 1.1 and 1.2 (without overflow fix).  
Display the original, lower contrast, and higher contrast images.

### Step 5:
Split the image (boy.jpg) into B, G, R components and display the channels

## Program Developed By:
```
- **Name:** clarissa k  
- **Register Number:**212224030047

  ### Ex. No. 01

# Import libraries
import cv2
import numpy as np
import matplotlib.pyplot as plt

# Read the image ('Eagle_in_Flight.jpg') using OpenCV imread() as a grayscale image.
eagle_gray = cv2.imread('Eagle_in_Flight.jpg', cv2.IMREAD_GRAYSCALE)

# Print the image width, height & Channel
height, width = eagle_gray.shape
print(f"Grayscale Image - Width: {width}, Height: {height}, Channels: 1 (Grayscale)")

# Display the image using matplotlib imshow().
plt.figure(figsize=(8,6))
plt.imshow(eagle_gray, cmap='gray')
plt.title('Grayscale Eagle Image')
plt.axis('off')
plt.show()

# Save the image as a PNG file using OpenCV imwrite().
cv2.imwrite('Eagle_output.png', eagle_gray)

# Read the saved image above as a color image using cv2.cvtColor().
eagle_color = cv2.imread('Eagle_output.png')
eagle_color_rgb = cv2.cvtColor(eagle_color, cv2.COLOR_BGR2RGB)

# Display the Colour image using matplotlib imshow() & Print the image width, height & channel
height_c, width_c, channels_c = eagle_color.shape
print(f"Color Image - Width: {width_c}, Height: {height_c}, Channels: {channels_c}")

plt.figure(figsize=(8,6))
plt.imshow(eagle_color_rgb)
plt.title('Color Eagle Image')
plt.axis('off')
plt.show()

# Crop the image to extract any specific (Eagle alone) object from the image.
# Assuming eagle is in the center (adjust coordinates as needed)
crop_y_start = height_c // 4
crop_y_end = height_c * 3 // 4
crop_x_start = width_c // 4
crop_x_end = width_c * 3 // 4
eagle_cropped = eagle_color[crop_y_start:crop_y_end, crop_x_start:crop_x_end]

# Resize the image up by a factor of 2x.
eagle_resized = cv2.resize(eagle_cropped, None, fx=2, fy=2, interpolation=cv2.INTER_LINEAR)

# Flip the cropped/resized image horizontally. (Eagle Image)
eagle_flipped = cv2.flip(eagle_resized, 1)

# Read in the image ('Apollo-11-launch.jpg').
apollo = cv2.imread('Apollo-11-launch.jpg')
apollo_rgb = cv2.cvtColor(apollo, cv2.COLOR_BGR2RGB)

# Add the following text to the dark area at the bottom of the image (centered on the image):
text = 'Apollo 11 Saturn V Launch, July 16, 1969'
font_face = cv2.FONT_HERSHEY_PLAIN
font_scale = 1.5
thickness = 2
text_color = (255, 255, 255)  # White text

# Get text size
text_size = cv2.getTextSize(text, font_face, font_scale, thickness)[0]
text_x = (apollo.shape[1] - text_size[0]) // 2
text_y = apollo.shape[0] - 30  # 30 pixels from bottom

# YOUR CODE HERE: use putText()
cv2.putText(apollo, text, (text_x, text_y), font_face, font_scale, text_color, thickness)

# Draw a magenta rectangle that encompasses the launch tower and the rocket.
# rect_color = magenta
magenta_color = (255, 0, 255)  # Magenta in BGR
# Adjust rectangle coordinates based on your image
rect_start = (100, 50)  # (x, y) top-left corner
rect_end = (400, apollo.shape[0] - 100)  # (x, y) bottom-right corner

# YOUR CODE HERE
cv2.rectangle(apollo, rect_start, rect_end, magenta_color, 3)

# Display the final annotated image.
apollo_final_rgb = cv2.cvtColor(apollo, cv2.COLOR_BGR2RGB)
plt.figure(figsize=(10,8))
plt.imshow(apollo_final_rgb)
plt.title('Apollo 11 Launch - Annotated')
plt.axis('off')
plt.show()

# Read the image ('boy.jpg').
boy = cv2.imread('boy.jpg')
boy_rgb = cv2.cvtColor(boy, cv2.COLOR_BGR2RGB)

# Adjust the brightness of the image.
# Create a matrix of ones (with data type float64)
# matrix_ones = 
# YOUR CODE HERE
matrix_ones = np.ones(boy.shape, dtype=np.uint8) * 50

# Create brighter and darker images.
# img_brighter = cv2.add(img, matrix)
# img_darker = cv2.subtract(img, matrix)
# YOUR CODE HERE
img_brighter = cv2.add(boy, matrix_ones)
img_darker = cv2.subtract(boy, matrix_ones)

# Convert to RGB for display
img_brighter_rgb = cv2.cvtColor(img_brighter, cv2.COLOR_BGR2RGB)
img_darker_rgb = cv2.cvtColor(img_darker, cv2.COLOR_BGR2RGB)

# Display the images (Original Image, Darker Image, Brighter Image).
plt.figure(figsize=(15,5))

plt.subplot(1,3,1)
plt.imshow(boy_rgb)
plt.title('Original Image')
plt.axis('off')

plt.subplot(1,3,2)
plt.imshow(img_darker_rgb)
plt.title('Darker Image')
plt.axis('off')

plt.subplot(1,3,3)
plt.imshow(img_brighter_rgb)
plt.title('Brighter Image')
plt.axis('off')

plt.tight_layout()
plt.show()

# Modify the image contrast.
# Create two higher contrast images using the 'scale' option with factors of 1.1 and 1.2 (without overflow fix)
# matrix1 = 
# matrix2 = 
# img_higher1 = 
# img_higher2 = 
# YOUR CODE HERE
# Convert to float to avoid overflow
boy_float = boy.astype(np.float32)
contrast1 = boy_float * 1.1
contrast2 = boy_float * 1.2

# Clip values to 0-255 and convert back to uint8
img_higher1 = np.clip(contrast1, 0, 255).astype(np.uint8)
img_higher2 = np.clip(contrast2, 0, 255).astype(np.uint8)

# For lower contrast (scale factor 0.8)
lower_contrast = np.clip(boy_float * 0.8, 0, 255).astype(np.uint8)

# Convert to RGB
img_higher1_rgb = cv2.cvtColor(img_higher1, cv2.COLOR_BGR2RGB)
img_higher2_rgb = cv2.cvtColor(img_higher2, cv2.COLOR_BGR2RGB)
lower_contrast_rgb = cv2.cvtColor(lower_contrast, cv2.COLOR_BGR2RGB)

# Display the images (Original, Lower Contrast, Higher Contrast).
plt.figure(figsize=(15,5))

plt.subplot(1,3,1)
plt.imshow(boy_rgb)
plt.title('Original Image')
plt.axis('off')

plt.subplot(1,3,2)
plt.imshow(lower_contrast_rgb)
plt.title('Lower Contrast (0.8x)')
plt.axis('off')

plt.subplot(1,3,3)
plt.imshow(img_higher2_rgb)
plt.title('Higher Contrast (1.2x)')
plt.axis('off')

plt.tight_layout()
plt.show()

# Split the image (boy.jpg) into the B,G,R components & Display the channels.
b, g, r = cv2.split(boy)

plt.figure(figsize=(15,5))

plt.subplot(1,3,1)
plt.imshow(b, cmap='gray')
plt.title('Blue Channel')
plt.axis('off')

plt.subplot(1,3,2)
plt.imshow(g, cmap='gray')
plt.title('Green Channel')
plt.axis('off')

plt.subplot(1,3,3)
plt.imshow(r, cmap='gray')
plt.title('Red Channel')
plt.axis('off')

plt.tight_layout()
plt.show()

# _merged the R, G, B , display along with orginal image
merged_bgr = cv2.merge([b, g, r])
merged_rgb = cv2.cvtColor(merged_bgr, cv2.COLOR_BGR2RGB)

plt.figure(figsize=(12,5))

plt.subplot(1,2,1)
plt.imshow(boy_rgb)
plt.title('Original Image')
plt.axis('off')

plt.subplot(1,2,2)
plt.imshow(merged_rgb)
plt.title('Merged BGR Image')
plt.axis('off')

plt.tight_layout()
plt.show()

# Split the image into the H, S, V components & Display the channels.
hsv_boy = cv2.cvtColor(boy, cv2.COLOR_BGR2HSV)
h, s, v = cv2.split(hsv_boy)

plt.figure(figsize=(15,5))

plt.subplot(1,3,1)
plt.imshow(h, cmap='hsv')
plt.title('Hue Channel')
plt.axis('off')

plt.subplot(1,3,2)
plt.imshow(s, cmap='gray')
plt.title('Saturation Channel')
plt.axis('off')

plt.subplot(1,3,3)
plt.imshow(v, cmap='gray')
plt.title('Value Channel')
plt.axis('off')

plt.tight_layout()
plt.show()

# _merged the H, S, V, display along with orginal image
merged_hsv = cv2.merge([h, s, v])
merged_hsv_rgb = cv2.cvtColor(merged_hsv, cv2.COLOR_HSV2RGB)

plt.figure(figsize=(12,5))

plt.subplot(1,2,1)
plt.imshow(boy_rgb)
plt.title('Original Image')
plt.axis('off')

plt.subplot(1,2,2)
plt.imshow(merged_hsv_rgb)
plt.title('Merged HSV Image')
plt.axis('off')

plt.tight_layout()
plt.show()
```
## Output:
<img width="1267" height="519" alt="image" src="https://github.com/user-attachments/assets/c374faee-3938-4e06-8fcc-e9fcc65a9066" />


## Result:
Thus, the images were read, displayed, brightness and contrast adjustments were made, and bitwise operations were performed successfully using the Python program.

