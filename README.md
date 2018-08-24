# Introduction
[Darknet](https://pjreddie.com/darknet/) encapsulates **object detection** and **image classification** models, which gives us a user-friendly interface to deploy advanced deep learning models.
Here I use Darknet to perform coral reef detection in inner hackathon.  
[The page](https://pjreddie.com/darknet/yolo/) gives detailed information about how to install Darknet, training YOLO on VOC or COCO datasets.  
The following steps following [Training YOLO on VOC](https://pjreddie.com/darknet/yolo/) part.


# Procedures
- [Build Darknet in GPU mode](https://blog.csdn.net/edogawachia/article/details/80999636)
  - If you don't know the **cuda path**, try `whereis nvcc`
  - [OpenCV error](https://github.com/pjreddie/darknet/issues/485)
- Prepare the training data YOLO needs
  - Refer to `https://pjreddie.com/media/files/voc_label.py`. For each image, there is a *txt* file contains several lines that format like this `<object-class> <x> <y> <width> <height>`. Each line represents a object in the image.
- Create three config files
  - Model configuration file: `darknet/cfg/your_file_name.cfg`
    - Copy `darknet/cfg/yolov3.cfg` to `darknet/cfg/your_file_name.cfg`, revise `Line 7, 8..` refer from [here](https://medium.com/@manivannan_data/how-to-train-yolov3-to-detect-custom-objects-ccbcafeb13d2)
  - Data configuration file: `darknet/cfg/your_file_name.data`
    - Copy `darknet/cfg/voc.data` to `darknet/cfg/your_file_name.data`, revise all lines. 
    - The **backup** folder stores trained models.
    - The **names** stores the labels of training data specified in the following **Data label file**.
  - Data label file: `darknet/data/your_file_name.names`
    - Copy `darknet/data/voc.names` to `darknet/data/your_file_name.names`, each row corresponding to one label.
- Train the model
  - Execute `./darknet detector train cfg/your_file_name.data cfg/your_file_name.cfg darknet53.conv.74`， where `darknet53.conv.74` is pretrained model weights.
- Evulate the model
  - Compute the `recall`: `./darknet detector recall data/your_file_name.names cfg/your_file_name.cfg your_model_x00.weights`.
  - If ERROR `data/coco_val_5k.list is not found` happens, [here](https://github.com/pjreddie/darknet/issues/326) is the solution.
- Use the model
  - Execute `./darknet detector test cfg/your_file_name.data cfg/your_file_name.cfg your_model_x00.weights your_test_image_path`.
  - The result of above command is `predictions.png`.
