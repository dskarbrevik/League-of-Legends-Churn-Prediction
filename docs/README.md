
## What is FoodHUD?

This is what it's about


## Background Info on the Idea

Millions of Americans struggle with their weight and its effects on their physical health. Portion control can be a serious challenge for busy people, and often it is difficult for someone to estimate the caloric content of a given meal. While a human may struggle with the task, a well-trained machine may not.

Recent advances in computer vision, driven by developments in deep learning, have enabled computers to successfully distinguish thousands of classes of images of items with impressive accuracy. Researchers have already developed image classification data sets comprised of images of food.

One such data set, Food101, contains 101 classes of food with 1000 high-resolution images for each class. Several well-established baselines exist for Food101, including relatively performant deep learning solutions. This provides a proof of concept that the automated identification of food is within our grasp, and these solutions may provide a path forward for Americans seeking to reign in their caloric intake.

## What we wanted to make

Inspired by the progress made on the Food101 data set, we set out to implement an automated tool to estimate the caloric content of a plate of food based on an image. We believe that such a product, available as a web-based application or preferably a mobile application, could bolster a consumer’s ability to control their diet and be more health- and nutrition-conscious.


## The things we did to make it

We developed our solution in multiple parallel stages. We knew that we needed to acquire not only labelled image data of foods, but also coinciding nutritional data on those same foods. We have already discussed Food101 as a data source, but we explored additional options to augment this source with more images. Knowing that we would need to integrate caloric data at a minimum, we search for and evaluated a few different sources of nutritional information. We chose to update the neural network architecture to reflect our augmented data set. Finally, we needed to create an application to leverage our deep learning model and perform class identification and use a lookup table to derive calorie prediction.

### Image Set Augmentation

Although Food101 offered a laudable 1000 images per class, we suspected that we would benefit from an augmented data set. To that end, we explored a few additional data sources, including pre-compiled data sets and public image sites. Namely, we considered integrating data from the UEC data set, which could have supplied more images and additional classes for us. However, these classes seemed skewed toward Japanese cuisine, which would have been less beneficial for a typical American dieter.

We also attempted to scrape images from Google Images, Instagram, and Flickr. We found a useful Chrome extension for Google Images, but the daily throughput was limited. Because of this, it would have taken a long time to acquire an image set even comparable to Food101. MORE STUFF ABOUT INSTAGRAM AND FLICKR

Ultimately, we decided that automated data augmentation of our existing image set was the best path forward for us. This would allow us to expand our image set from the existing Food101 data set without worrying about data quality and accuracy. Therefore, we produced 10 images from every 1 original image in Food101, taking 5 different crops of the image in its original orientation and also its reflection about its vertical axis. This expanded image set proved to improve our performance over the baseline, and so we were satisfied with it.

### Nutritional Information Acquisition

Because we wanted to predict calorie data about a food, we needed a reliable and open resource for nutritional information. Initially, we sought to use MyFitnessPal as a data source. It seemed like a reliable data source, and all of the Food101 categories had at least one relevant entry in the nutritional database. However, due to MyFitnessPal’s policy against automated collection of data, we chose to collect it by hand, which was cumbersome. Because of this, we only took one data point for each category, which resulted in quite a few categories that had anomalous calorie values. These anomalies, as well as the fact that MyFitnessPal is not a truly open resource, led us to discard MyFitnessPal as a data source.

Fortunately, we found a reasonable public data source in the USDA National Nutrient Database for Standard Reference. This provides nutrient data about nearly 9000 foods, many of which overlap with the food categories in Food101. Using the caloric information from this data source, we were able to compute the median caloric density (kcal/100g) for 53 of the original 101 food categories in Food101. These calorie values seem reasonable and will allow us to integrate nutritional data into our application.

### Architecture Modification



## What came out in the end

### Web-Based Application

Using Flask and Google Cloud services, we cr

### Regression Experiment

Our final architecture used classification and then looked up the median caloric density based on the class. However, we were also interested in determining the performance of the model when attempting to use linear regression to predict the caloric density directly. We ran an experiment with a subset of 1000 of the Food101 images, filtering down to those images belonging to the 53 classes for which we had caloric densities. Then, resizing the images, we collected features for each image from the Keras application for the VGG16 network trained on ImageNet. We then fed these features into a PCA with a number of principal components varying from 1 to 100 and used a 5-fold cross-validation to compute a mean squared error. We used a much simpler model as our baseline, whose only input features were the sums across all pixels of each of the three color channels. This three-feature model performed comparably with models trained on the PCA transformation of the extracted image features, which suggests that, at the very least, linear regression is not a reasonable option for direct regression of caloric density.

Mobile Application

