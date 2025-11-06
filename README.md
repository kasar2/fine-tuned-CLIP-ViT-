# fine-tuned-CLIP-ViT-


                                                                                           📄 **Project Summary**

Zero-Shot CLIP ViT for Image Blur DetectionThis project focused on solving the core problem of determining if an image is perceptually blurry or sharp. We leveraged the power of multimodal pre-trained models, specifically CLIP (Contrastive Language-Image Pre-training), which demonstrated superior zero-shot capabilities for our specific use case, minimizing the need for extensive initial labeling.

                                                                                             **💡 The Problem**
Binary Image Quality ClassificationThe primary objective was to build a robust and efficient classification system to categorize images into one of two classes: Blurry or Sharp. A traditional approach would require a large, fully labeled dataset for initial training; however, the zero-shot capabilities of CLIP offered a significant advantage in rapid prototyping and high baseline performance.

                                                                            **🤖 Architecture & Zero-Shot Foundation (CLIP ViT)**
The foundational model used was CLIP (Contrastive Language-Image Pre-training), developed by OpenAI.CLIP ArchitectureCLIP is a multimodal model composed of two distinct encoders trained simultaneously:

**Image Encoder (ViT-L/14):** This component processes the visual input. We specifically utilized the Vision Transformer (ViT) architecture. ViT treats an image as a sequence of patches, similar to how a Transformer processes words, allowing it to capture global dependencies.

**Text Encoder (Transformer):** This component processes the corresponding text caption.

These encoders are trained to maximize the cosine similarity between the correct image-text pairs in a massive dataset, aligning the image and text representations in a shared latent space.

**Zero-Shot Functionality**

The initial success of CLIP stemmed from its zero-shot transferability. For image blur detection, this meant:

**Text Encoding:** Encoding descriptive class labels into text embeddings (e.g., "A photograph of a sharp image," "A photograph of a blurry image").
**Image Encoding:** Encoding the input image into an image embedding.
**Classification:** Calculating the cosine similarity between the image embedding and all class text embeddings. The class with the highest similarity score is chosen as the prediction.

This zero-shot method provided a strong, immediate baseline, validating the approach for the blur detection task without requiring a single training step on our custom data.

                                                                        **⚙️ Fine-Tuning Strategy:Achieving Maximum Performance**

While zero-shot was powerful, fine-tuning on our proprietary custom data was essential to maximize accuracy and tailor the model to the specific characteristics of our image distribution.

**Descriptive Points on Fine-Tuning:**

**Necessity of Fine-Tuning:** Although CLIP offered high zero-shot performance, fine-tuning was crucial to overcome the domain gap between the massive, general-purpose training data (used by CLIP) and the specific nature of our blur detection task.

**Targeted Layer Unfreezing:** Amongst the many hyperparameter experiments, the most successful approach was unfreezing only the last two layers of the Vision Transformer (ViT) Image Encoder.

Efficiency: This strategy significantly reduced the number of trainable parameters compared to unfreezing the entire model, leading to faster training times and requiring less GPU memory.

Feature Preservation: By keeping the initial, deeper layers frozen, we effectively preserved the highly abstract, general-purpose feature representations learned from CLIP's initial training (e.g., object recognition, texture).

Task Specificity: Unfreezing the final layers allowed the model to learn subtle, task-specific features related to image degradation (blur, noise, artifacts) and to adjust the final classification head for optimal decision-making on our binary classes.

**Hyperparameter Optimization:** 
Extensive experimentation was conducted with key hyperparameters:

**Learning Rate (LR):** Testing a range of lower LRs (e.g., $1e-5, 5e-6$) was necessary to ensure that the pre-trained weights were adjusted incrementally and carefully, avoiding catastrophic forgetting.

**Epochs & Patience:** Various epoch counts and early stopping mechanisms (Patience) were tested to find the sweet spot that allowed for convergence without overfitting to the small custom dataset.

**Resulting Model:** This selective fine-tuning process resulted in a model that retained strong general visual knowledge while developing highly specialized, high-accuracy decision boundaries for blur detection.

                                                                                          **✅ Conclusion**

The adoption of the Zero-Shot CLIP ViT provided a cutting-edge solution for our image blur detection problem. The strategic decision to selectively fine-tune the final two layers balanced the need for high-performance specialization with the efficiency of retaining powerful pre-trained knowledge, yielding the best model performance across all approaches tested.
