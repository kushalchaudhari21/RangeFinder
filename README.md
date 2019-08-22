## Description

This is an python script to get a colorspace range for your computer vision or image processing projects using trackbars.
The demo can be found in folder [demo](https://github.com/kushalchaudhari21/RangeFinder/tree/master/demo).

## Sample Invocation

In the terminal, you need to specify a filter and "only one" image source: 
```
(python) range-detector --filter RGB --image /path/to/image.png
```
You can also capture image via webcam to process it further:
```
(python) range-detector --filter HSV --webcam
```
Example:
```console
python range_finder.py --filter HSV --image /Users/Desktop/tutorials/git/RangeFinder/img.jpeg 
```
![executionCommand](https://github.com/kushalchaudhari21/RangeFinder/blob/master/demo/SampleInvocation.gif)

## Usage

**1.**  Execute the script and the program starts printing real-time trackbar values.

**2.** Next, two windows pop up on the screen containing original image and trackbar filtered image respectively.

**3.** Adjust the trackbar sliders to get the desired region of interest(ROI). In this case, trackbars are adjusted to detect the red LED blob in the image.

![UsingTrackbarstoSelectBlob](https://github.com/kushalchaudhari21/RangeFinder/blob/master/demo/UsingTrackbarstoSelectBlob.gif) 

**4.** Note down the corresponding colourspace ranges for your ROI.

![correspondingHSVforBlob](https://github.com/kushalchaudhari21/RangeFinder/blob/master/demo/correspondingHSVforBlob.gif)

## Important Insights

* Function *adjust_gamma* can be used to preprocess the image to attenuate effect of different lighting conditions.
```python
def adjust_gamma(image, gamma):

   invGamma = 1.0 / gamma
   table = np.array([((i / 255.0) ** invGamma) * 255
      for i in np.arange(0, 256)]).astype("uint8")

   return cv2.LUT(image, table)
```
* ROI range for a different colourspace can be evaluated by converting image to that colourspace instead of RGB or HSV.
```python
if args['image']:
        image = cv2.imread(args['image'])
        image = cv2.resize(image, (768,576))
        #image = adjust_gamma(image,gamma)
        #image = cv2.addWeighted(image,alpha,np.zeros(image.shape,image.dtype),-0.3,beta)
        
        if range_filter == 'RGB':
            frame_to_thresh = image.copy()
        else:
        #Converting to LAB colourspace to get LAB ranges. Sequence remains the same for ranges.
            frame_to_thresh = cv2.cvtColor(image, cv2.COLOR_BGR2LAB)
    else:
        camera = cv2.VideoCapture(0)
```
