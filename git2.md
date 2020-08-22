# Keras 中I/O方法小引


本文idea**来自于** [Jason Brownlee 大神](https://machinelearningmastery.com/author/jasonb/) 的keras入门博客，原文：
https://machinelearningmastery.com/how-to-load-convert-and-save-images-with-the-keras-api/


## 正文
keras有大量不在文档内的I/O方法

完成本教程后，您将知道：

* 如何使用Keras API加载和显示图像。
* 如何使用Keras API将加载的图像转换为NumPy数组并转换回PIL格式。
* 如何使用Keras API将加载的图像转换为灰度图像并将其保存到新文件。



**1. example image process**

main API class [ImageDataGenerator class](https://keras.io/preprocessing/image/)

除了IDG类，还有大量的Keras API

> 具体来说，Keras提供了用于加载，转换和保存图像数据的功能。这些函数在utils.py函数中，并通过[image.py](https://github.com/keras-team/keras/blob/master/keras/preprocessing/image.py)模块公开。

keras所有图像处理都需要安装Pillow库



**How to Load an Image with Keras**

`load_img() ` function: load image from **file** as a **PIL image object**

``` python
from keras.preprocessing.image import load_img
import PIL
img = load_img('.//cifar-10//train//1.png')
print(type(img))
print(img.size)
print(img.format)
print(img.mode)
img.show()
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
<class 'PIL.PngImagePlugin.PngImageFile'>
(32, 32)
PNG
RGB
```

keras中`load_img` 存储于 [utils,py](https://github.com/keras-team/keras-preprocessing/blob/master/keras_preprocessing/image/utils.py) 中，其中定义了诸多additional arguments:

```python
def load_img(path, grayscale=False, color_mode='rgb', target_size=None,
             interpolation='nearest'):
    """Loads an image into PIL format.
    # Arguments
        path: Path to image file.
        grayscale: DEPRECATED use `color_mode="grayscale"`.
        color_mode: The desired image format. One of "grayscale", "rgb", "rgba".
            "grayscale" supports 8-bit images and 32-bit signed integer images.
            Default: "rgb".
        target_size: Either `None` (default to original size)
            or tuple of ints `(img_height, img_width)`.
        interpolation: Interpolation method used to resample the image if the
            target size is different from that of the loaded image.
            Supported methods are "nearest", "bilinear", and "bicubic".
            If PIL version 1.1.3 or newer is installed, "lanczos" is also
            supported. If PIL version 3.4.0 or newer is installed, "box" and
            "hamming" are also supported.
            Default: "nearest".
    # Returns
        A PIL Image instance.
    # Raises
        ImportError: if PIL is not available.
        ValueError: if interpolation method is not supported.
    """
```



`img_to_array()` function: from **PIL** img format to **NumPy array**

``` python
from keras.preprocessing.image import load_img, img_to_array
import PIL
img = load_img('.//cifar-10//train//1.png')
print(type(img))
img_array = img_to_array(img)
print(img_array.dtype)
print(img_array.shape)
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
<class 'PIL.PngImagePlugin.PngImageFile'>
float32
(32, 32, 3)
```

`img_to_array()` 被定义在 [image.py](https://github.com/keras-team/keras-preprocessing/blob/master/keras_preprocessing/image/utils.py#L77) 中