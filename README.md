Croppic
=======

croppic is an image cropping jquery plugin that will satisfy your needs and much more. croppic is supported on current browsers, such as chrome, firefox, IE, safari and opera.

**NOTE**: This fork contains additional Ajax POST payload:

- `imgRelativeX`
- `imgRelativeY`
- `cropRelativeW`
- `cropRelativeH`

This data can be used to crop the original (size) image. For example:

    // Path to the uploaded image
    $imgUrl = $_POST['imgUrl'];

    // Relative to the original size of the image
    $imgRelativeY = $_POST['imgRelativeY'];
    $imgRelativeX = $_POST['imgRelativeX'];
    $cropRelativeH = $_POST['cropRelativeH'];
    $cropRelativeW = $_POST['cropRelativeW'];

    /**
     * Crop the original image in relation to the scaled version (using Imagick)
     */
    $imagick = new \Imagick();
    $imagick->readImage(realpath($imgUrl));
    $imagick->cropImage($cropRelativeW, $cropRelativeH, $imgRelativeX, $imgRelativeY);
    $imagick->setImageFormat("jpeg");
    $imagick->writeImage("temp/croppedOriginalImg_" . rand() . ".jpeg");


Documentation
---------------------

### INITIALISATION

Simplest implementation. Beware that you must provide width and height of the container.

#### JS

    var cropperHeader = new Croppic('yourId');

#### HTML

    <div id="yourId"></div>

#### CSS

    #cropContainerHeader {
        width: 200px;
        height: 150px;
        position:relative; /* or fixed or absolute */
    }

### UPLOAD URL

Path to your img upload proccessing file.

#### JS

    var cropperOptions = {
        uploadUrl:'path_to_your_image_proccessing_file.php'
    }

    var cropperHeader = new Croppic('yourId', cropperOptions);

You will be receiving the image file via AJAX POST method as multipart/form-data; 
(note that ajax is limited to same domain requests)

[Download php example file](https://raw.githubusercontent.com/rawswift/croppic/master/img_save_to_file.php)


After successful image saving, you must return the following json. 
( image width and height required for limiting max zoom, so images dont come out blurry.)

    {
        "status":"success",
        "url":"path/croppedImg.jpg"
    }

In case of error response

    {
        "status":"error",
        "message":"your error message text"
    }

### UPLOAD DATA

Additional data you want to send to your image upload proccessing file

#### JS

    var cropperOptions = {
            uploadUrl:'path_to_your_image_proccessing_file.php',
            uploadData:{
            "dummyData":1,
            "dummyData2":"text"
        }
    }

    var cropperHeader = new Croppic('yourId', cropperOptions);

### CROP URL

Path to your img crop processing file.

#### JS

    var cropperOptions = {
        cropUrl:'path_to_your_image_cropping_file.php'
    }
    
    var cropperHeader = new Croppic('yourId', cropperOptions);

You will be receiving the following data via AJAX POST method as multipart/form-data (note that ajax is limited to same domain requests)

    imgUrl      // your image path (the one we recieved after successfull upload)
            
    imgInitW    // your image original width (the one we recieved after upload)
    imgInitH    // your image original height (the one we recieved after upload)

    imgW        // your new scaled image width
    imgH        // your new scaled image height

    imgX1       // top left corner of the cropped image in relation to scaled image
    imgY1       // top left corner of the cropped image in relation to scaled image

    cropW       // cropped image width
    cropH       // cropped image height

    imgRelativeX    // top left corner of cropped image in relation to original image
    imgRelativeY    // top left corner of cropped image in relation to original image

    cropRelativeW   // cropped image width in relation to original image
    cropRelativeH   // cropped image height in relation to original image


[Download php example file](https://raw.githubusercontent.com/rawswift/croppic/master/img_crop_to_file.php)

After successful image saving, you must return the following json. ( image width and height required for limiting max zoom, so images dont come out blurry. )

    {
        "status":"success",
        "url":"path/croppedImg.jpg"
    }

In case of error response

    {
        "status":"error",
        "message":"your error message text"
    }

### CROP DATA

Additional data you want to send to your image crop proccessing file

#### JS

    var cropperOptions = {
        customUploadButtonId:'path_to_your_image_proccessing_file.php',
        cropData:{
            "dummyData":1,
            "dummyData2":"text"
        }
    }

    var cropperHeader = new Croppic('yourId', cropperOptions);

### Preload Image

Load an Image which is already on the Server

#### JS

    var cropperOptions = {
            cropUrl:'path_to_your_image_cropping_file.php',
            loadPicture:'path-to-your-image'
    }

    var cropperHeader = new Croppic('yourId', cropperOptions);

### Controls

- `doubleZoomControls`: Adds two extra zoom controls for 10 x `zoomFactor` zoom *(default is true)*
- `zoomFactor`: Zooms the image for the value in pixels *(default is 10)*
- `rotateControls`: Adds two extra rotate controls for left and right rotation *(default is true)*
- `rotateFactor`: Rotates the image for the value *(default is 5)*

#### JS

    var cropperOptions = {
            zoomFactor:10,
            doubleZoomControls:true,
            rotateFactor:10,
            rotateControls:true         
    }

    var cropperHeader = new Croppic('yourId', cropperOptions);

### OUTPUT URL

After successful image cropping, cropped img url is set as value for the input containing the outputUrlId

#### HTML

    <input type="text" id="myOutputId">

#### JS

    var cropperOptions = {
        outputUrlId:'myOutputId'
    }

    var cropperHeader = new Croppic('yourId', cropperOptions);

### CUSTOM UPLOAD BUTTON

If you want a custom upload img button, just like in the first example in [http://www.croppic.net/](http://www.croppic.net/)

#### JS

    var cropperOptions = {
        customUploadButtonId:'myDivId'
    }

    var cropperHeader = new Croppic('yourId', cropperOptions);


### MODAL

If you want to open the cropping in modal window (default is false)

#### JS

    var cropperOptions = {
        modal:true
    }

    var cropperHeader = new Croppic('yourId', cropperOptions);

### LOADER HTML

If you want to open the cropping in modal window (default is false) **THE WRAPPING DIV MUST CONTAIN "LOADER" CLASS**

#### JS

    var cropperOptions = {
        loaderHtml:'<img class="loader" src="loader.png" >'
    }

    var cropperHeader = new Croppic('yourId', cropperOptions);

### Process Inline

If you want to handle the Initial Fileupload in Javascript(Filereader) rather then on Server side (default is false) **NOT ALL BROWSERS SUPPORT THE FILEREADER API: EXAMPLE IE10+ IS SUPPORTED**

#### JS

    var cropperOptions = {
        processInline:true,
    }

    var cropperHeader = new Croppic('yourId', cropperOptions);

### IMG EYECANDY

Transparent image showing current img zoom **TIP**: to prevent layout breaking, set the parent div of the cropper to `"overflow":"hidden"`

#### JS

    var cropperOptions = {
        imgEyecandy:true,
        imgEyecandyOpacity:0.2
    }

    var cropperHeader = new Croppic('yourId', cropperOptions);

### CALLBACKS

Some callbacks ( open your console and watch the output as you mess arround with the example cropper )

#### JS

    var cropperOptions = {
        onBeforeImgUpload: function() {
            console.log('onBeforeImgUpload');
        },
        onAfterImgUpload: function() {
            console.log('onAfterImgUpload');
        },
        onImgDrag: function() {
            console.log('onImgDrag');
        },
        onImgZoom: function() {
            console.log('onImgZoom');
        },
        onBeforeImgCrop: function() {
            console.log('onBeforeImgCrop');
        },
        onAfterImgCrop: function() {
            console.log('onAfterImgCrop');
        },
        onReset: function() {
            console.log('onReset');
        },
        onError: function(errormsg) {
            console.log('onError:'+errormsg);
        }
    }

    var cropperHeader = new Croppic('yourId', cropperOptions);

### METHODS

#### JS

    var cropper = new Croppic('yourId', cropperOptions);

    cropper.destroy()   // no need explaining here :) 
    cropper.reset()     // destroys and then inits the cropper

Examples / Demo
---------------------

Visit [http://www.croppic.net/](http://www.croppic.net/)

Browser compatibility
---------------------

Croppic works in most major browsers (yes, now even in IE9):

- **IE8**: Untested
- **IE9**: Works!
- **IE10+**: Works!
- **Safari 4+**: Works!
- **Firefox 3+**: Works!
- **Chrome 14+**: Works!
- **Opera 15+**: Works!

Support
-------

Please don't contact me by email for support. The right place to get support is here:
[Croppic Issues](https://github.com/sconsult/croppic/issues) 


MIT LICENCE
-----------

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sub-license, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
