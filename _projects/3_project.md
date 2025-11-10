---
layout: page
title: Machine Learning(CSCI 5525) Project
description: Collaborated with Daniel Miao and Zaifu Zhan
img: assets/img/MLproject/2layers_CNN_cover.png
importance: 3
category: work
---

According to the law of large numbers, the sample mean approximates the distribution expectation as the sample size increases. Hoeffding inequality [learningfromdata] shows that the upper bound of the probability of the difference between the sample mean $$\bar{x}$$ and the overall mean $$\mu$$ is greater than $$\epsilon$$ is $$2 \exp (-2 n \epsilon^{2})$$.

However, even now, in the era of data explosion, there is no guarantee that there will be enough data for all AI problems.  
For example, in medical image processing, to solve very rare lesions, the sample size is really not enough. Sometimes we even lose access to data because of copyright issues. Thus, we can only design a small sample learning algorithm based on the data we have. How to improve the learning model's accuracy against limited data is a necessary problem we have to deal with. Moreover, this study is also meaningful for improving the robustness of learning algorithms.

$$
P(|\bar{x} - \mu| \geq \epsilon) \leq 2 \exp \left(-2 n \epsilon^{2}\right)
$$

However, even in the era of data abundance, many AI tasks suffer from limited data. For example, rare medical conditions yield few training samples, and copyright restrictions often limit data access. Improving model performance under limited data is essential for both accuracy and robustness.

Small datasets increase the risk of overfitting—models with low bias and high variance may learn noise rather than signal. Several strategies help mitigate this issue:

- **Data Augmentation:** Generates additional training data via geometric transforms, color adjustments, image mixing, GANs, and more.
- **Ensemble Learning:** Combines multiple models to reduce variance and improve generalization.
- **Transfer Learning:** Leverages pretrained models as starting points for new tasks with limited data.

This study evaluated several image classification techniques on the Kaggle 100 Sports dataset. The ensemble method combining three pretrained models (AlexNet, MobileNetv2, and ResNet) achieved the highest accuracy, benefiting from leveraging the strengths of each model. However, this method is computationally expensive compared to single pretrained models. Simple CNNs performed poorly and overfitted, while transfer learning with pretrained networks consistently outperformed them due to effective feature extraction from large-scale training.

Data augmentation improves generalization but has limitations. Although ensemble methods enhance accuracy, their computational cost is high. Exploring less expensive ensemble techniques like snapshot learning or boosting could be worthwhile but may sacrifice performance. Overall, transfer learning remains a promising, efficient, and accurate approach for image classification. Future work could focus on developing theoretical frameworks and systematic pipelines to better guide transfer learning across diverse domains.


