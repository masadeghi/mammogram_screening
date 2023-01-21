<!-- Improved compatibility of back to top link: See: https://github.com/othneildrew/Best-README-Template/pull/73 -->
<a name="readme-top"></a>
<!--
*** Thanks for checking out the Best-README-Template. If you have a suggestion
*** that would make this better, please fork the repo and create a pull request
*** or simply open an issue with the tag "enhancement".
*** Don't forget to give the project a star!
*** Thanks again! Now go create something AMAZING! :D
-->



<!-- PROJECT SHIELDS -->
<!--
*** I'm using markdown "reference style" links for readability.
*** Reference links are enclosed in brackets [ ] instead of parentheses ( ).
*** See the bottom of this document for the declaration of the reference variables
*** for contributors-url, forks-url, etc. This is an optional, concise syntax you may use.
*** https://www.markdownguide.org/basic-syntax/#reference-style-links
-->
[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![LinkedIn][linkedin-shield]][linkedin-url]


<h3 align="center">Detecting breast cancer in screening mammograms</h3>

  <p align="center">
    Based on the RSNA mammogram cancer detection competition on Kaggle
    <br />
    <br />
    <a href="https://github.com/masadeghi/mammogram_screening"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="https://github.com/masadeghi/mammogram_screening/issues">Report Bug</a>
    ·
    <a href="https://github.com/masadeghi/mammogram_screening/issues">Request Feature</a>
  </p>
</div>



<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#next-steps">Next Steps</a></li>
      </ul>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li><a href="#contributing">Contributing</a></li>
    <li><a href="#license">License</a></li>
    <li><a href="#contact">Contact</a></li>
  </ol>
</details>



<!-- ABOUT THE PROJECT -->
## About The Project

In this project, I've used mammograms from the [RSNA mammogram cancer detection competition on Kaggle](https://www.kaggle.com/competitions/rsna-breast-cancer-detection) to build my model. This dataset contains DICOM files of several mammograms (left and right breast; CC and MLO views, with some patients having additional views) in addition to information on age, breast density, the machine used for the mammogram, etc. in a tabular format.

This dataset poses several challenges (as identified following exploratory data analysis) that need to be addressed for a model to have high precision:

1. **Image heterogeneity:** Different machines produce different scans. Essentially, two of the machines produce scans with a white background (maximum pixel intensity) while the rest produce images with a black background (minimum pixel intensity).
2. **Small region of interest (ROI) compared to the whole scan:** Most of the area of each scan is blank space. These areas should be cut out to focus on the ROI.
3. **Imbalanced classes:** The most significant challenge is the unbalanced nature of the classes, with only ~ 2% of the scans being cancerous. In contrast, machine learning algorithms train best when there are equal proportions of each class.
4. **Performance limitations:** While kaggle notebooks provide significant processing capabilities in a variety of GPUs and TPUs, they also have some limitations. For example, the runtime for each notebook is limited to 12 hours. In addition, the competition rules dictate a maximum running time of 9 hours without any accelerators for running inference on the hidden test set. Finally, the dataset contains 54706 images which pose a significant computational burden. All this requires significant code optimization and performance tuning in the model. 

Currently, this repo contains only the base model I've constructed to tackle these challenges in the ras_mammogram_training.ipynb script. The model consists of an efficientnetB4 architecture from Timm with a custom classifier layer. To address the challenges:
1. **Challenges 1 & 2:** The code first preprocesses the images to make the backgrounds uniform, min-max normalizes the pixel intensities, and crops out the ROI for further analysis.
2. **Challenge 3:** While there are several, significantly better, solutions for the third challenge (unbalanced data), I've chosen undersampling in this script to save on time and computation. Importantly, however, I haven't performed random undersampling which may lead to the loss of predictive information from the dataset. Instead, I've used the mammogram of the healthy breast as the control for the cancerous breast. This keeps the distribution of all other features constant, helping the model to focus only on the things that differentiate cancer mammograms from healthy ones.
3. **Challenge 4:** Finally, I've used the joblib and concurrent.futures modules to enable multiprocessing during image processing. Also, I've implemented automatic mixed precision casting during model training.

<p align="right">(<a href="#readme-top">back to top</a>)</p>



### Next Steps
The model has signficant room for improvement:
1. **Improving the way we handle the class imbalance in the data:** Undersampling deprives us of large amounts of data. Instead, we could use additional image augmentation for upsampling the minority class, or we could modify the training algorithm to give higher penalties to the misclassifications of the minority class.
2. **Tuning the learning rate scheduler:** The OneCycleLr scheduler results in exploding gradients (as observed from nan validation losses). Tuning the scheduler could speed up and improve training.
3. **Finding the optimum threshold:** In the base model, I've used 0.5 as the threshold for classifying model outputs as cancer or healthy. However, we could extract better performance through finding the optimum threshold.
4. **Gradient accumulation during training:** The current batch size of 16 is really small. To enable training with larger batch_sizes (e.g., 64) we could implement gradient accumulation and adjust the gradients every few (e.g., 4) steps.
5. **Testing other classifier heads and other models:** In the base model, I've built an arbitrary head consisting of two hidden layers and one output node.
6. **Fine-tuning the whole model rather than the classifier layer.**
7. **Incorporating the tabular data in the model to enhance prediction.**

<p align="right">(<a href="#readme-top">back to top</a>)</p>



### Built With

* Python
* PyTorch
* Timm

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- CONTRIBUTING -->
## Contributing

Contributions are what make the open source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue with the tag "enhancement".
Don't forget to give the project a star! Thanks again!

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- LICENSE -->
## License

Distributed under the GPL-3.0 license. See `LICENSE.txt` for more information.

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- CONTACT -->
## Contact

Amin Sadeghi - masadeghi6@gmail.com

Project Link: [https://github.com/masadeghi/mammogram_screening](https://github.com/masadeghi/mammogram_screening)

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[contributors-shield]: https://img.shields.io/github/contributors/masadeghi/mammogram_screening.svg?style=for-the-badge
[contributors-url]: https://github.com/masadeghi/mammogram_screening/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/masadeghi/mammogram_screening.svg?style=for-the-badge
[forks-url]: https://github.com/masadeghi/mammogram_screening/network/members
[stars-shield]: https://img.shields.io/github/stars/masadeghi/mammogram_screening.svg?style=for-the-badge
[stars-url]: https://github.com/masadeghi/mammogram_screening/stargazers
[issues-shield]: https://img.shields.io/github/issues/masadeghi/mammogram_screening.svg?style=for-the-badge
[issues-url]: https://github.com/masadeghi/mammogram_screening/issues
[license-shield]: https://img.shields.io/github/license/masadeghi/mammogram_screening.svg?style=for-the-badge
[license-url]: https://github.com/masadeghi/mammogram_screening/blob/master/LICENSE.txt
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://www.linkedin.com/in/mohammad-amin-sadeghi-md/
