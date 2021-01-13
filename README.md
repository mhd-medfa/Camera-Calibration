# Camera-Calibration

The goal is to find the intrinsic and extrinsic parameters on
camera on Xiaomi Mi A1 mobile phone. before getting started I disabled autofocus mode of the camera.
I made 31 photos of chessboard.

The size of square on the chessboard I used is 15 mm. The key is that we will know each square size and we will assume each square is equal. It means, that now we know the coordinates of each point on the chess plane. After that we can use the formulas to define intrinsic and extrinsic parameters

<a href="">
  <img align="center" alt="" width="540px" src="./chessboard/Chessboard_to_be_printed_on_A4_paper.png" />
</a>


<a href="">
  <img align="center" alt="" width="540px" src="./test/IMG_20201001_133037.jpg" />
</a>

### Calibration code

The chessboard is a 9x6 matrix so we set our width=9 and height=6. These numbers are the intersection points square corners met. “Criteria” is our computation criteria to iterate calibration function. 

objpoints is the map we use for the chessboard. imgpoints is a matrix that holds chessboard corners in the 3D world. These coordinates are coming from the pictures we have taken.

We have a for loop to iterate over the images. cv2.imread gets the image and cv2.cvtColor changes it to grayscale. cv2.findChessboardCorners gets the points and we already have the points.

If the function returns successfully we can start to interpolate. 
The last step, use calibrateCamera function and read the parameters then we save the camera matrix to a yml file.


<a href="">
  <img align="center" alt="" width="220px" src="./dataset/IMG_20201001_132924.jpg" />
</a>
<a href="">
  <img align="center" alt="" width="220px" src="./dataset/IMG_20201001_132945.jpg" />
</a>


### Estimating the height and width of the Rubik's Cube

Now since we have calibrated our camera, and stored intrinsic and extrinsic parameters. I took a photo of a rubik's cube using the calibrated camera, and we want to estimate the height and width of the selected object using both a ruler and an image from the calibrated camera. 

Now we need to find the corners coordinates of rubik's cube


To do that we need to either do some image processing or to hardcoded corners into an array by cheating the points from the image. Well I chose the long way for the sake of having some fun

I tried to use different color spaces (RGB, HSV, YUV, and LAB) and I found LAB is a good choice in our case

<a href="">
  <img align="center" alt="" width="540px" src="https://i.ibb.co/8Ks4b1g/rgb2lab.png" />
</a>

### Undistort the Image

Use the cameraParameters object to remove lens distortion from the image. This is necessary for accurate measurement.

<a href="">
  <img align="center" alt="" width="540px" src="https://i.ibb.co/Cbhf2Y2/undistort-image.png" />
</a>

Just to make the problem easier temporarily we will eliminate the padding area

<a href="">
  <img align="center" alt="" width="540px" src="https://i.ibb.co/M2XvYSb/eliminate-padding.png" />
</a>

We see that a* layer (a*: Red/Green Value) of Lab image gives us good result

<a href="">
  <img align="center" alt="" width="540px" src="https://i.ibb.co/dGsrHCw/A-layer-of-lab-image.png" />
</a>

To find the contour of the object we will use the closing morphological transform

<a href="">
  <img align="center" alt="" width="540px" src="https://i.ibb.co/FzRcvHp/find-contour-with-closing-morph-transform.png" />
</a>

**Duh! why we still have a gray scale image let's convert it to binary image to make our calculations faster and even having better results**


<a href="">
  <img align="center" alt="" width="540px" src="https://i.ibb.co/RYjJycz/gray2binary.png" />
</a>

Let's use cv2.goodFeaturesToTrack to get corners of the object and we need to calculate the new chessboard corners, then let's visualize the corners to check the quality of result

<a href="">
  <img align="center" alt="" width="540px" src="https://i.ibb.co/B4x99xt/result.png" />
</a>

Well that's not bad, I think it's enough for now since we have the ability to calculate the sides of the rubik's cube and block size of chessboard

To calculate the block size of chessboard we could sort the corners by euclidean distance and calculate the difference between them over the unmatched axis

For the cube we will calculate two sides of it (they should be almost equal to each other)

Rubik's cube dimensions using ruler are 5.5 cm for each side  and the result we got is 3.78 cm by 3.54 cm


### If you like what I do and you want to support me:

Bitcoin address: <a href="bc1q5cjffml32qrvks3xd0hyau8jx3gf5cd04hrn77">
  <img align="left" alt="Bitcoin" width="22px" src="https://raw.githubusercontent.com/mhd-medfa/mhd-medfa/main/assets/bitcoin.svg.png" />
</a>
<mark>`bc1q5cjffml32qrvks3xd0hyau8jx3gf5cd04hrn77`</mark>

Litecoin address: <a href="ltc1q9l4dr8jdtcakhe8qekep9lfwpgscpllxv5zy27">
  <img align="left" alt="Litecoin" width="22px" src="https://raw.githubusercontent.com/mhd-medfa/mhd-medfa/main/assets/litecoin.svg.png" />
</a>
<mark>`ltc1q9l4dr8jdtcakhe8qekep9lfwpgscpllxv5zy27`</mark>

Tether address: <a href="0x0006a16f43D0fdf480bCc88D4398Fe73D6806fc9"> 
  <img align="left" alt="TetherUSD" width="22px" src="https://raw.githubusercontent.com/mhd-medfa/mhd-medfa/main/assets/tether.svg" />
</a>
<mark>`0x0006a16f43D0fdf480bCc88D4398Fe73D6806fc9`</mark>
