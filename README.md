# HAMK Digital Bioeconomy Drone Image Data Analysis
To support the decision making in agricultural research, one may want to observe the evolution of vegetation from the high-altitude image. In particular, when utilizing the high-altitude drone images, we face the problem of low image resolution (in this instance, one pixel contains `4x4cm`, whereas the image is taken from `80m` altitude), from which we can not derive the vegetation coverage within one pixel:
![highalt.png](highalt.png) <br>
At the same time, we also have a small set of close-up calibration images, in which we can clearly observe whether the pixel is a vegetation or not, because the resolution is much higher:
![closeup.png](closeup.png) <br>
We propose the following pipeline to infer the vegetation cover from the original high-altitude image:
![dataPipeline.jpg](dataPipeline.jpg) <br>
Using the close-up calibration image, we label true-only instances of vegetation and soil (without edge labelling):
![maskedImg.png](maskedImg.png) <br>
Then we use the classification and RGB color values of the pixels as dependent and independent variables respectively as inputs to the logistic regression. Then, we can label the model's predictions and thus infer the label mask and the vegetation cover:
![maskedOverImg.png](maskOverImg.png) <br>
The following stages of the approach are currently being researched and modified. The general gist of the next steps is:
 - Adapt Generative Adversarial Networks to generate close-up images:
     - We take tensorflow pix2pix as a starting point: https://www.tensorflow.org/tutorials/generative/pix2pix. It uses the label mask as the input for generation of facades;
     - However, we do not have the capacity to create a label mask for each of the close-up images to perform the training;
     - Therefore, we replace the label mask with a random noise vector as an input for the GAN;
     - Currently, the predictions of this approach look as follows:
     - ![loop.gif](loop.gif)<br>
 - Subject the generated close-up images to the same pipeline used to calculate vegetation in the original close-up;
 - Produce the high-altitude image from the generated close-ups, using several options. We intend to generate in the close-ups in the manner that they obtain some kind of continuity. Therefore, when we attach the images together, and reduce the resolution, we obtain a synthetic high-altitude image, which will look somewhat naturally;
 - As the synthetic image consists of the generated close-up images, for which we already know the vegetation cover, we can thus assess the vegetation cover in the synthetic high-altitude image. Moreover, we can use both the synthetic image and the calculated vegetation cover as an input to the model, which will predict the vegetation cover in the high-altitude image;
 - Thus, we are able to infer the vegetation cover from the original high-altitude image by using this model.

## Installations

### Anaconda
The biggest part of the research is conducted with Miniconda. You can find the installation instructions here: https://docs.conda.io/en/latest/miniconda.html. The correct version of the required Miniconda dependencies are contained in the `requirements.txt`. Notice that running a single notebook may not mandate the installation of all requirements.

### Label-studio
The labelling of close-up image was performed with the open source software label-studio. It can be performed with any relevant labelling software, however, the code is intended to work with json files which are produced in the specific format attributable to label-studio. For the instructions of installation, please check: https://labelstud.io/guide/#Quick-start.

### Enabling GPU
Generating images is a resource-consuming process for the hardware, and thus we suggest taking use of the GPU. If you are not using Google Colab, and running the notebooks locally, you would need to install the dependencies in the order they are presented in `requirements.txt`. Also, it is mandatory that you have the latest GPU drivers. You can check whether your GPU is enabled for tensorflow calculations by running the first cell in the notebooks where it is recommended. If the GPU is detected successfully, you will be able to read the name of your GPU.

### License
The original image data is not licensed under the MIT license. It is derived from Häme University of Applied Sciences images (https://doi.org/10.23729/d083d6ad-aa68-4826-8792-7a169e2d2dd9), licensed under [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/). For more details on the rights of use, check the LICENSE file.