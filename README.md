## Diabetic-Retinopathy-Detection
The main emphasis of this application lies in extracting crucial segments from the retina fundus and subsequently determining whether Diabetic Retinopathy is present or not.

## Table of Contents
- Introduction
- About Dataset
- Reading fundus images from folder
- Preprocessing
- Segmentation
- Feature Extraction
- Classification Model
- Results

## Introduction
Diabetic Retinopathy (DR) is a serious complication of diabetes that affects the eyes, potentially leading to vision impairment or even blindness if left untreated. Timely detection and intervention are crucial in preventing the progression of this condition. The "Diabetic-Retinopathy-Detection" project aims to address this challenge by leveraging advanced image processing and machine learning techniques.
The primary goal of our application is to analyze retina fundus images and identify signs of Diabetic Retinopathy. By extracting crucial segments from these images and employing a robust classification model, we can provide a reliable tool for early diagnosis and intervention. Early detection allows for timely medical assistance, helping to mitigate the impact of Diabetic Retinopathy on patients' vision.
This project not only contributes to the field of medical diagnostics but also aligns with the broader objective of leveraging technology for proactive healthcare. The automated detection provided by our application has the potential to assist healthcare professionals in their diagnosis, making the process more efficient and accessible.
As we move forward, the following sections will delve into the technical aspects of the project, including the folder structure, preprocessing steps, feature extraction techniques, classification models, and the achieved results. We invite contributors and users to explore, collaborate, and enhance the capabilities of this Diabetic Retinopathy detection system.

## About Dataset
The "Diabetic-Retinopathy-Detection" project utilizes the Indian Diabetic Retinopathy Detection dataset, a comprehensive collection of retina fundus images curated for the purpose of advancing research in the field of diabetic eye diseases. This dataset is specifically tailored for the identification and analysis of Diabetic Retinopathy in the Indian population.
Link: https://idrid.grand-challenge.org/Data/

## Reading Fundus images from folder
To train and evaluate the Diabetic-Retinopathy-Detection model, it is essential to efficiently read and preprocess the fundus images from the dataset. In this section, we will guide you through the process of reading images from the dataset folder and preparing them for further analysis.
IDRid dataset is divided into three folders- Segmentation, Disease Grading and Localization. In this project we have utilised Segmentaion and Disease Grading folders which also consists of groundtruths and labelled csv files.

## Preprocessing
The input fundus images in RGB format are first resized to fit a maximum of 1500 pixels in width and 1152 pixels in height while maintaining the aspect ratio. After extracting a particular channel (green or red) we apply CLAHE to enhance the features for segmentation and feature extraction. Gamma correction is applied to segment microaneurysm . Gamma correction adjusts the brightness and contrast of an image
Median Filter is first applied to remove the salt and pepper noise which are recurring bright and dark spots. In order for the segmentation to be accurate it is necessary to remove the salt and pepper noise. After the median filter is applied, we apply a Gaussian filter to smoothen the image and for blurring for better segmentation results
The red channel of the resized RGB image is extracted to segment the optic disc, the green channel is extracted to segment the blood vessels and the microaneurysm. The image is converted to gray-scale image for segmented the exudates.

## Segmentation
The segmentation of the fundus image consists of four segments- optic disc, microaneurysm, blood vessels.

- Optic Disc:
After extracting the red channel and applying the CLAHE, we apply binary thresholding(choosing an appropriate local threshold). We apply thresholding and after that we set all the pixel values above 245 to 255(white) and the pixel value below 245 to 0(black). We obtain a binary image after thresholding and to this we apply morphological opening on the binary image and then morphological closing. Next step is contouring the closed optic disc which are the boundaries of connected regions. Since the optic disc is the largest area we sort the contour area from largest to smallest. The largest contour is drawn on this mask and it is filled to obtain the optic disc

- Microaneurysm:
Microaneurysm are tiny dots in a retina that are very hard to segment. The process that we have employed here is to segment the image using thresholding and morphological operations. Initially after the extraction of the green channel and applying CLAHE, adjust gamma is applied which allows contrast enhancement by mapping the pixel values to the corrected gamma values and then applies the transformation using the LUT function of cv2. After this thresholding and morphological operations such as tophat and opening are applied.The resulting binary mask highlights the region of interest.

- Blood vessels:
The process of segmentation of blood vessels initially starts with the green channel and applying CLAHE to enhance the blood vessels. Subsequently various operations such as median, filtering, subtraction of the original image and the green channel enhanced image followed by blurring,thresholding(by choosing an appropriate local threshold), morphological operation-opening and then the exact operations are performed with slight variations in the parameters. Both these images are then combined using the bitwise OR operator. The result is a binary image with highlighted blood vessels 

- Exudates
Exudates are typically the bright lesions in a fundus image. The process of extraction of these exudates initially starts with the conversion of the image to gray scale and then the average pixel intensity of the entire image is calculated. Based on the average pixel intensity thresholding is applied to segment the exudates. After thresholding, the centroid of the white pixels are calculated that of the white pixels. Then further using these centroid values we remove pixels i.e., if the centroid y-coordinate is less than 1000 then the pixels near the top are removed and similarly if the centroid’s x-coordinate is greater than 2200 or zero then pixels near the right
side are removed or otherwise pixels to the left are removed

- Feature Extraction:
In DR detection this involvesarea of optic disc, ratio of microaneurysm, length of blood vessels, Blood Vessel Tortuosity, edge density, mean intensity, standard deviation, exudate density. Even extraction of all the potential features, it is necessary to only use the ones that are relevant and give a higher accuracy. The area of the optic disc is calculated by counting the number of white pixels. The ratio of microaneurysm is calculated by dividing the number of white pixels by the total number of pixels in the image. The length of the blood vessels are calculated by skeletonizing the blood vessels. The blood vessel tortuosity is calculated by dividing the perimeter of the blood vessel by the length of blood vessels. The edge density is calculated by detecting the edges using the Canny operator and then finding the density. Mean intensity and standard deviation are very straightforward to calculate by using the formulae offered by the numpy library. Finally exudate density is calculated in a way similar to edge density

## Classification:
The next step after the feature extraction is to use the necessary features to classify the fundus images into whether a person has Diabetic Retinopathy or not. The model used for the binary classification is Random Forest after it gave the highest accuracy compared to KNN Classifier, Logistic Regression, SVM and Gradient Boosting. The features used for model training are ratio of microaneurysm, blood vessel length, blood vessel tortuosity and mean intensity. The other features were not considered due the fact that the features reduced the accuracy of the model.

## Deployement:
https://github.com/Ramitha-V/Detection-of-Diabetic-Retinopathy-using-Image-Processing-/assets/162662008/59cc751d-9f27-475f-a649-a5c67a5bb752

## Results:
The final result is segmented fundus region along with the classification of fundus as Diabetic Retinopathy or not.

## Link
https://diabetic-retinopathy-detection-vrvr.streamlit.app/?utm_medium=social

## Link to our blog
https://medium.com/@ramithavimala/detection-of-diabetic-retinopathy-using-image-processing-585dd37c1680
