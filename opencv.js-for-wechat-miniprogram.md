# opencv.js-for-wechat-miniprogram

opencv.js compile for wechat miniprogram

- this is the a tutorials for how to build opencv.js for wechat mini program
- also in how to customization your opencv.js

## first build your environment

1. **Ubuntu or IOS**  

   open your commond (cmd) run this code to init our opencv compile env

   ```shell
   sudo apt-get update
   sudo apt-get install python2.7
   sudo apt-get install nodejs
   sudo apt-get install build-essential
   sudo apt-get install cmake
   sudo apt-get install git-core
   sudo apt-get install default-jre
   sudo apt-get install doxygen   # must be install 
   sudo apt-get install nodejs
   sudo apt-get install npm
   ```

   

2. **install Emscripten**

   use cd command to enter your home directory 

   ```powershell
   # Enter Home directory
   cd ~
   # Get the emsdk repo or downlowad emsdk by Gitee
   git clone https://github.com/juj/emsdk.git
   # Enter emsdk directory
   cd emsdk
   # Fetch the latest version of the emsdk (not needed the first time you clone)
   git pull
   # Download and install the latest SDK tools.
   
   ./emsdk install latest
   ./emsdk activate latest
   ```

3. **activate source in your build env**

   ```shell
   source ./emsdk_env.sh
   ```

## build your opencv

##### 	1.clone opencv  and run your project with commond

```shell
# Get the OpenCV repo or gitee to download Opencv 
git clone https://github.com/opencv/opencv.git
# Enter CV directory
cd opencv
# build 
# JS version necessary
python ./platforms/js/build_js.py build_js
# Web version not necessary
python ./platforms/js/build_js.py build_wasm --build_wasm   # not necessary you can pass it
# Documents not necessary
python ./platforms/js/build_js.py build_js --build_doc      # not necessary you can pass it
# Tests necessary
python ./platforms/js/build_js.py build_js --build_test 
```

if you just to run first command , and dont know how to make opencv.js work , must need to run the build_test and run it ，then you will know



##### 	**2.How to make  opencv.js work**

cd to build_js/bin and you will see **opencv.js opencv_js.js** and add this code to your opencv.js On the **last line**

```js
var Module = {
  preRun: [],
  postRun: [] ,
  onRuntimeInitialized: function() {
    console.log("cv is ready");
    if (window.cv instanceof Promise) {   // importance all cv factory from here
      window.cv.then((target) => {
         window.cv = target;
      })
    } else {
      // for backward compatible
      console.log(cv.getBuildInformation());
    }
  },
  print:[] ,
  printErr: function(text) {
    console.error(text);
  },
  setStatus: function(text) {
    console.log(text);
  },
  totalDependencies: 0
};
Module.onRuntimeInitialized();
```

## Customize your OpenCV

1. ##### edit the config

    cd to ./platforms/js/ and you will find build_js.py file and config.py (config.py was not available before version 3.4.9)

   edit the config.py and Comment out any functions you don't need like this

   ```python
   core = {
       '': ['norm', 
           # 'absdiff', 'add', 'addWeighted', 'bitwise_and', 'bitwise_not', 'bitwise_or', 'bitwise_xor', 'cartToPolar',
           # 'compare', 'convertScaleAbs', 'copyMakeBorder', 'countNonZero', 'determinant', 'dft', 'divide', 'eigen',
           # 'exp', 'flip', 'getOptimalDFTSize','gemm', 'hconcat', 'inRange', 'invert', 'kmeans', 'log', 'magnitude',
           # 'max', 'mean', 'meanStdDev', 'merge', 'min', 'minMaxLoc', 'mixChannels', 'multiply', 
           # 'normalize',
           # 'perspectiveTransform', 'polarToCart', 'pow', 'randn', 'randu', 'reduce', 'repeat', 'rotate', 'setIdentity', 'setRNGSeed',
           # 'solve', 'solvePoly', 'split', 'sqrt', 'subtract', 'trace', 'transform', 'transpose', 'vconcat',
           # 'setLogLevel', 'getLogLevel',
       ],
       'Algorithm': [],
   }
   imgproc = {'': ['cvtColor', 'resize',
                   # 'Canny', 'GaussianBlur', 'Laplacian', 'HoughLines', 'HoughLinesP', 'HoughCircles', 'Scharr','Sobel', \
                   # 'adaptiveThreshold','approxPolyDP','arcLength','bilateralFilter','blur','boundingRect','boxFilter',\
                   # 'calcBackProject','calcHist',
                   # 'circle',
                   # 'compareHist','connectedComponents','connectedComponentsWithStats', \
                   # 'contourArea', 
                   # 'convexHull', 'convexityDefects', 'cornerHarris','cornerMinEigenVal','createCLAHE', \
                   # 'createLineSegmentDetector',
                   
                   # 'demosaicing','dilate', 'distanceTransform','distanceTransformWithLabels', \
                   # 'drawContours','ellipse','ellipse2Poly','equalizeHist','erode', 'filter2D', 'findContours','fitEllipse', \
                   # 'fitLine', 'floodFill','getAffineTransform', 'getPerspectiveTransform',
                   
                   # 'getStructuringElement', \
                   # 'goodFeaturesToTrack','grabCut','initUndistortRectifyMap', 'integral','integral2', 'isContourConvex', 'line', \
                   # 'matchShapes', 'matchTemplate','medianBlur', 'minAreaRect', 'minEnclosingCircle', 'moments', 'morphologyEx', \
                   # 'pointPolygonTest', 'putText','pyrDown','pyrUp','rectangle','remap', 
                   
                   # 'sepFilter2D','threshold', \
                   # 'undistort','warpAffine','warpPerspective','warpPolar','watershed', \
                   # 'fillPoly', 'fillConvexPoly', 'polylines',
       ],
       #'CLAHE': ['apply', 'collectGarbage', 'getClipLimit', 'getTilesGridSize', 'setClipLimit', 'setTilesGridSize'],
       # 'segmentation_IntelligentScissorsMB': [
       #     'IntelligentScissorsMB',
       #     'setWeights',
       #     'setGradientMagnitudeMaxLimit',
       #     'setEdgeFeatureZeroCrossingParameters',
       #     'setEdgeFeatureCannyParameters',
       #     'applyImage',
       #     'applyImageFeatures',
       #     'buildMap',
       #     'getContour'
       # ],
   }
   objdetect = {'': [
                       # 'groupRectangles'
                       ],
               #  'HOGDescriptor': ['load', 'HOGDescriptor', 'getDefaultPeopleDetector', 'getDaimlerPeopleDetector', 'setSVMDetector', 'detectMultiScale'],
               #  'CascadeClassifier': ['load', 'detectMultiScale2', 'CascadeClassifier', 'detectMultiScale3', 'empty', 'detectMultiScale'],
               #  'QRCodeDetector': ['QRCodeDetector', 'decode', 'decodeCurved', 'detect', 'detectAndDecode', 'detectMulti', 'setEpsX', 'setEpsY']
               }
   video = {'': ['calcOpticalFlowPyrLK',
               # 'CamShift', 'calcOpticalFlowFarneback', 
               
               #  'createBackgroundSubtractorMOG2', \
               #  'findTransformECC', 'meanShift'
                ],
           #  'BackgroundSubtractorMOG2': ['BackgroundSubtractorMOG2', 'apply'],
           #  'BackgroundSubtractor': ['apply', 'getBackgroundImage']
            }
   dnn =  {
           # 'dnn_Net': ['setInput', 'forward'],
          '': [
                   # 'readNetFromCaffe', 'readNetFromTensorflow', 'readNetFromTorch', 'readNetFromDarknet',
                   # 'readNetFromONNX', 'readNet', 'blobFromImage'
               ]
           }
   features2d = {
               #     'Feature2D': ['detect', 'compute', 'detectAndCompute', 'descriptorSize', 'descriptorType', 'defaultNorm', 'empty', 'getDefaultName'],
               #   'BRISK': ['create', 'getDefaultName'],
               #   'ORB': ['create', 'setMaxFeatures', 'setScaleFactor', 'setNLevels', 'setEdgeThreshold', 'setFirstLevel', 'setWTA_K', 'setScoreType', 'setPatchSize', 'getFastThreshold', 'getDefaultName'],
               #   'MSER': ['create', 'detectRegions', 'setDelta', 'getDelta', 'setMinArea', 'getMinArea', 'setMaxArea', 'getMaxArea', 'setPass2Only', 'getPass2Only', 'getDefaultName'],
               #   'FastFeatureDetector': ['create', 'setThreshold', 'getThreshold', 'setNonmaxSuppression', 'getNonmaxSuppression', 'setType', 'getType', 'getDefaultName'],
               #   'AgastFeatureDetector': ['create', 'setThreshold', 'getThreshold', 'setNonmaxSuppression', 'getNonmaxSuppression', 'setType', 'getType', 'getDefaultName'],
               #   'GFTTDetector': ['create', 'setMaxFeatures', 'getMaxFeatures', 'setQualityLevel', 'getQualityLevel', 'setMinDistance', 'getMinDistance', 'setBlockSize', 'getBlockSize', 'setHarrisDetector', 'getHarrisDetector', 'setK', 'getK', 'getDefaultName'],
                 # 'SimpleBlobDetector': ['create'],
               #   'KAZE': ['create', 'setExtended', 'getExtended', 'setUpright', 'getUpright', 'setThreshold', 'getThreshold', 'setNOctaves', 'getNOctaves', 'setNOctaveLayers', 'getNOctaveLayers', 'setDiffusivity', 'getDiffusivity', 'getDefaultName'],
               #   'AKAZE': ['create', 'setDescriptorType', 'getDescriptorType', 'setDescriptorSize', 'getDescriptorSize', 'setDescriptorChannels', 'getDescriptorChannels', 'setThreshold', 'getThreshold', 'setNOctaves', 'getNOctaves', 'setNOctaveLayers', 'getNOctaveLayers', 'setDiffusivity', 'getDiffusivity', 'getDefaultName'],
               #   'DescriptorMatcher': ['add', 'clear', 'empty', 'isMaskSupported', 'train', 'match', 'knnMatch', 'radiusMatch', 'clone', 'create'],
               #   'BFMatcher': ['isMaskSupported', 'create'],
                 '': [
                       #'drawKeypoints', 'drawMatches', 'drawMatchesKnn'
                       ]}
   photo = {'': [
                   # 'createAlignMTB', 'createCalibrateDebevec', 'createCalibrateRobertson', \
                   # 'createMergeDebevec', 'createMergeMertens', 'createMergeRobertson', \
                   # 'createTonemapDrago', 'createTonemapMantiuk', 'createTonemapReinhard', 'inpaint'
                 ],
           # 'CalibrateCRF': ['process'],
           # 'AlignMTB' : ['calculateShift', 'shiftMat', 'computeBitmaps', 'getMaxBits', 'setMaxBits', \
           #               'getExcludeRange', 'setExcludeRange', 'getCut', 'setCut'],
           # 'CalibrateDebevec' : ['getLambda', 'setLambda', 'getSamples', 'setSamples', 'getRandom', 'setRandom'],
           # 'CalibrateRobertson' : ['getMaxIter', 'setMaxIter', 'getThreshold', 'setThreshold', 'getRadiance'],
           # 'MergeExposures' : ['process'],
           # 'MergeDebevec' : ['process'],
           # 'MergeMertens' : ['process', 'getContrastWeight', 'setContrastWeight', 'getSaturationWeight', \
           #                   'setSaturationWeight', 'getExposureWeight', 'setExposureWeight'],
           # 'MergeRobertson' : ['process'],
           # 'Tonemap' : ['process' , 'getGamma', 'setGamma'],
           # 'TonemapDrago' : ['getSaturation', 'setSaturation', 'getBias', 'setBias', \
           #                   'getSigmaColor', 'setSigmaColor', 'getSigmaSpace','setSigmaSpace'],
           # 'TonemapMantiuk' : ['getScale', 'setScale', 'getSaturation', 'setSaturation'],
           # 'TonemapReinhard' : ['getIntensity', 'setIntensity', 'getLightAdaptation', 'setLightAdaptation', \
           #                      'getColorAdaptation', 'setColorAdaptation']
           }
   aruco = {'': [
               # 'detectMarkers', 'drawDetectedMarkers', 'drawAxis', 'estimatePoseSingleMarkers', 'estimatePoseBoard', 'estimatePoseCharucoBoard', 'interpolateCornersCharuco', 'drawDetectedCornersCharuco'
               ],
           # 'aruco_Dictionary': ['get', 'drawMarker'],
           # 'aruco_Board': ['create'],
           # 'aruco_GridBoard': ['create', 'draw'],
           # 'aruco_CharucoBoard': ['create', 'draw'],
           # 'aruco_DetectorParameters': ['create']
           }
   calib3d = {'': ['solvePnP', 
             
                   ]}
   white_list = makeWhiteList([core, imgproc, objdetect, video, dnn, features2d, photo, aruco, calib3d])
   ```

   

2. ##### rebuild your program

   reference to build opencv (Make sure your build_js file or build_wasm is empty) 

   and you will get file in build_js/bin or build_wasm/bin 



## build opencv for wechat

1. ##### check file 

   firts build your opencv and test it work by commond (build__test, run test file and make sure your order function is work)

2. ##### change code to adapt the environment

    wechat not support keyword like gloabl or window ,so we need to change some content , Thank for https://github.com/leo9960/opencv.js-wechat/commits?author=leo9960 contribution his solution can great run opencv.js under SE6 。

   

   if you want to use opencv in SE5 environment, you need change lot of code 

   # TBC  

