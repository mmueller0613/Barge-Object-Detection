# Barge-Object-Detection
A model trained on YOLOv5 to detect and label objects on a barge or in a shipyard. It is meant to be installed on and run on a Nvidia Jetson Orin Nano

## How it works

This project uses a YOLOv5 object detection model trained to find different objects on a barge. The model looks at an input image and finds things like containers, cranes, laptops, and more.

  - It takes an image and runs it through the model.

  - The model returns a list of boxes and labels for what it sees.

  - The script then draws boxes and labels on the image and saves the result.

## Instructions
**1. Download Model and Config Files**

Download the following files and place them in a new folder:

    bargedetectionv3.pt

    data.yaml

**2. Install Dependencies**
```
git clone https://github.com/ultralytics/yolov5.git
cd yolov5
pip install -r requirements.txt
pip install torch torchvision torchaudio
```
**3. Prepare the Project Folder**

Create a folder called bargedetection in your home directory:
```
mkdir ~/bargedetection
```
Move your model and config files into that folder:
```
mv bargedetectionv3.pt ~/bargedetection/
mv data.yaml ~/bargedetection/
```
Create a folder to hold your input images:
```
mkdir ~/bargedetection/images
```
Place the images you want to run the model on into the images/ folder.

Your file structure should look like this:

![image](https://github.com/user-attachments/assets/5e7dfdf5-4f18-45c4-8c9f-9b5ba5a84bcb)


**4. Run the Model**
```
python detect.py --weights ~/bargedetection/bargedetectionv3.pt --data ~/bargedetection/data.yaml --source ~/bargedetection/images/ --img 640 --conf 0.2 --project ~/bargedetection --name output
```
The output images will be saved in ~/bargedetection/output/.

If too many objects are being falsely detected, increase the confidence to a higher percentage (ex: --conf 0.4)

If too few objects are being detected, decrease the confidence level to a lower percentage (ex --conf 0.1)

**5. Improve Label Visibility (Optional)**

To increase label font size and thickness:

  - Open yolov5/detect.py

  - Find this line (around line 278):
```
annotator.box_label(xyxy, label, color=colors(c, True))
```
  - Replace it with the following block:
```
xyxy = [int(x.item()) for x in xyxy]
cv2.rectangle(im0, (xyxy[0], xyxy[1]), (xyxy[2], xyxy[3]), colors(c, True), thickness=line_thickness)
if label:
    font_scale = 6.0
    font_thickness = 20
    text_size = cv2.getTextSize(label, cv2.FONT_HERSHEY_SIMPLEX, font_scale, font_thickness)[0]
    text_origin = (xyxy[0], xyxy[1] - 10 if xyxy[1] - 10 > 10 else xyxy[1] + 20)
    cv2.putText(im0, label, text_origin, cv2.FONT_HERSHEY_SIMPLEX, font_scale, colors(c, True), font_thickness)
```
This is how it should look when you are done:

![image](https://github.com/user-attachments/assets/83cd2b0e-00ec-4ee7-9ff8-ffbaaef28594)


This allows you to customize how large and thick the text labels appear in the output images.
