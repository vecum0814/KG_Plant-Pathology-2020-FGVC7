# KG_Plant-Pathology-2020-FGVC7
Identify the category of foliar diseases in apple trees

<img width="952" alt="스크린샷 2022-02-04 오전 1 00 36" src="https://user-images.githubusercontent.com/52812351/152379685-a7242dca-e251-446f-8d05-d9e17dd1fb7c.png">

In this kaggle competition, I had to distinguish between leaves which are healthy, those which are infected with apple rust, those that have apple scab, and those with more than one disease.

<img width="1343" alt="스크린샷 2022-02-04 오전 1 02 47" src="https://user-images.githubusercontent.com/52812351/152380088-d2d2cd03-48ab-4a3a-a7e2-7b3eb273914e.png">
The dataset has following characteristics:
+ Individual image size is (1365, 2048).
+ Most of the leafs with diseases have a spot on them, with a various colour.
+ Most of the leafs to diagnose are center-located

Hence, in terms of data augmentation, I decided not to use methods like cutout(which creates a dot-shaped noise on image) and color changing filters. In opposite, I decided to use center-zoom method.

First, I used a pretrained Xception model with input of (224, 224) and ReduceLRonPlateau learning rate scheduler.
<img width="813" alt="스크린샷 2022-02-04 오전 1 15 41" src="https://user-images.githubusercontent.com/52812351/152382451-559bc5fd-2309-4bfa-9272-1908454c461d.png">

After this first attemp, I tried to keep the ratio of the original image (1365, 2048) whilst keep in mind of the risk of Out of Memory issue. I selected the image size of (320, 512) for this attempt and used Ramp up and Step decay learning rate scheduler.
<img width="882" alt="스크린샷 2022-02-04 오전 1 21 30" src="https://user-images.githubusercontent.com/52812351/152383611-0fe37ef0-d465-48a1-a977-02dc164f7599.png">
It is shown that the re-scaling the image size and its ratio and the change of learning rate scheduler definitely made some difference.

I changed the pretrained model from Xception to EfficientNetB3, B5, and B7. EfficientNet model requires more GPU RAM than Xception, so I changed the batch size.

<img width="752" alt="스크린샷 2022-02-04 오전 1 25 15" src="https://user-images.githubusercontent.com/52812351/152384523-7123ffa3-b24f-4a86-b293-15ee068d16ea.png">

I tried to keep the input shape expected for each EfficientNet model. So B3 has size image size of (320, 512), B5 and B7 has (456, 456) of image size.
For the last EfficientNetB7 model, I did not split the train data into train dataset and validation dataset. I just used all of the training dataset to train my image, and just used Ramp up and Step decay method to keep control the learning rate per epochs.

Here is the score of my models.
<img width="933" alt="스크린샷 2022-02-04 오전 1 29 57" src="https://user-images.githubusercontent.com/52812351/152385336-fb032a8a-89ea-4626-b0eb-0a37e6d390fd.png">
