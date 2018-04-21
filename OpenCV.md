[TOC]

## cvLoadImage

函数原型：IplImage* cvLoadImage( const char* filename, int flags=CV_LOAD_IMAGE_COLOR )

filename：待读入文件的文件名（含后缀）

flags：指定图像的颜色和深度

指定的颜色可将输入图片转为 3 信道(CV_LOAD_IMAGE_COLOR)，单信道(CV_LOAD_IMAGE_

GRAYSCALE)，或保持不变(CV_LOAD_IMAGE_ANYCOLOR)

指定的深度可将输入图片转为每颜色信道每像素 8 比特，或保持不变

载入最真实的图像，选择 CV_LOAD_IMAGE_ANYDEPTH | CV_LOAD_IMAGE_ANYCOLOR



## cvCreateImage

函数原型：IplImage* cvCreateImage(CvSize cvSize(int width, int height), int depth, int channels)

创建图像，并分配存储空间

depth：图像像素的位深度

channels：每个像素的通道数，可以为 1,2,3 或 4。channels 是交叉存储的



## cvZero

新建图像后，使用 cvZero() 函数，将每个像素都置为 0，即全黑



## cvSetImageROI

函数原型：void cvSetImageROI（IplImage* **image**，CvRect **rect**）

基于给定的矩形设置图像的 ROI（感兴趣区域，region of interesting）

rect：矩形



## cvCopy

函数原型：void cvCopy( const CvArr* src, CvArr* dst, const CvArr* mask=NULL )

把 src 中的数据复制到 dst 的内存中



## cvResetImageROI

函数原型：void cvResetImageROI（IplImage* image）

释放图像中被设定的感兴趣区域



## cvNamedWindow

函数原型：int cvNamedWindow( const char* name, int flags )

name：窗口的名字，被显示为窗口标题

flags：窗口属性标志。目前唯一支持的标志是 CV_WINDOW_AUTOSIZE，不可手动改变窗口大小，它会自动调整以适合被显示图像



## cvShowImage

函数原型：void cvShowImage( const char* name, const CvArr* image  )

name：显示图像的窗口的名字

image：图像的指针 



## cvSaveImage

函数原型：int cvSaveImage( const char*  filename, const CvArr* image )

filename：保存的文件名

image：图像的指针



## cvWaitKey

在一个给定的时间内（单位为 ms），等待用户按键触发。如果在该段时间内，用户按下某个键，函数返回这个键的 ASCII 码，否则返回 0。

用它可以实现视频的暂停和开始功能



## cvReleaseImage

函数原型：void cvReleaseImage( IplImage** image )

释放图像的内存空间



## cvDestroyWindow

函数原型：void cvDestroyWindow( const char* name )

销毁窗口

name：待销毁窗口名字







