# HAMK Digital Bioeconomy Drone Image Data Analysis
## Project Progression
<p align="justify">To support the decision making in agricultural research, one may want to observe the evolution of vegetation from the high-altitude image. In particular, when utilizing the high-altitude drone images, we face the problem of low image resolution (in this instance, one pixel contains `4x4cm`, whereas the image is taken from `80m` altitude), from which we can not derive the vegetation coverage within one pixel.</p>
<p align="center"><img src="highalt.png")</p><br>
<p align="justify">At the same time, we also have a small set of close-up calibration images, in which we can clearly observe whether the pixel is a vegetation or not, because the resolution is much higher:</p>
<p align="center"><img src="closeup.png"></p>
<p align="justify">We propose the following pipeline to infer the vegetation cover from the original high-altitude image:</p>
<p align="center"><img src="dataPipeline.jpg"></p>
<p align="justify">Using the close-up calibration image, we label true-only instances of vegetation and soil (without edge labelling):</p>
<p align="center"><img src="maskedImg.png")</p>
<p align="justify">Then we use the classification and RGB color values of the pixels as dependent and independent variables respectively as inputs to the logistic regression. Then, we can label the model's predictions and thus infer the label mask and the vegetation cover:</p>
<p align="center"><img src="maskOverImg.png")</p></br>

- Adapt Generative Adversarial Networks to generate close-up images:
  - We take [tensorflow pix2pix](https://www.tensorflow.org/tutorials/generative/pix2pix) as a starting point. It uses the label mask as the input for generation of facades;
  - However, we do not have the capacity to create a label mask for each of the close-up images to perform the training;
  - Therefore, we replace the label mask with a random noise vector as an input for the GAN;

<p align="justify">The following stages of the approach are currently being researched and modified. The general gist of the next steps is:</p>

- <p align="justify">Subject the generated close-up images to the same pipeline used to calculate vegetation in the original close-up;</p>
- <p align="justify">Produce the high-altitude image from the generated close-ups, using several options. We intend to generate in the close-ups in the manner that they obtain some kind of continuity. Therefore, when we attach the images together, and reduce the resolution, we obtain a synthetic high-altitude image, which will look somewhat naturally;</p>
- <p align="justify">As the synthetic image consists of the generated close-up images, for which we already know the vegetation cover, we can thus assess the vegetation cover in the synthetic high-altitude image. Moreover, we can use both the synthetic image and the calculated vegetation cover as an input to the model, which will predict the vegetation cover in the high-altitude image;</p>
- <p align="justify">Thus, we are able to infer the vegetation cover from the original high-altitude image by using this model.</p>

## Installations

For installation instructions, check our [Wiki](https://github.com/hamk-uas/drone-image-analysis/wiki).

## License
The original image data is not licensed under the MIT license. It is derived from Häme University of Applied Sciences images (https://doi.org/10.23729/d083d6ad-aa68-4826-8792-7a169e2d2dd9), licensed under [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/). For more details on the rights of use, check the [license](https://github.com/hamk-uas/drone-image-analysis/blob/main/LICENSE).
