# Cartoonization-with-OPENCV
import cv2
import numpy as np

num_down = 2           #number of downsampling steps 
num_bilaberal = 7      # number of bilateral filtering steps 

img_rgb = cv2.imread("file:///C:/Users/amalm/Downloads/20200219180707_7B3A5270.JPG")
print(img_rgb.shape)    #print the dimension of the picture 
#resizing so as to get optimal results after un sampling is done 
img_rgb=cv2.resize(img_rgb,(800,800))

#downsample image using Gaussian Pyramid 
img_color=img_rgb
for _ in range (num_down):
    img_color = cv2.bilateralFilter(img_color, d=9,sigmaColor=9, sigmaSpace=7)

#upsample image to orginal size 
for _ in range(num_down):
    img_color = cv2.pyrUp (img_color)

img_gray = cv2.cvtColor(img_rgb, cv2.COLOR_RGB2GRAY)
img_blur = cv2.medianBlur(img_gray, 7)

img_edge = cv2.adaptiveThreshold(img_blur,255,cv2.ADAPTIVE_THRESH_MEAN_C,cv2.THRESD_BINARY,blockSize=9,C=2)

#convert back to color, bit-AND with color image 
img_edge = cv2.cvtColor(img_edge, cv2.COLOR_GRAY2RGB)
img_cartoon = cv2.bitwise_and(img_color,img_edge)

#display
#cv2.imshow("cartoon",img_cartoon)
stack=np.hstack([img_rgb,img_cartoon])
cv2.imshow('Stacked Images',stack)
