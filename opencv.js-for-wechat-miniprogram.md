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

4. **download opencv and compile**

    

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

5. **How to make  opencv.js work**

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

   

6. asd

7. asd

8. 



