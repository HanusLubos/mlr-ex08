#Report of Task 4, exercise 8
Hanus Luboš, 256983
----------------------------
1. The full comparison can be found in third point, showing how each variant improved or lowered the classification accuracy and relation to the noise. The best combination is the pretrained MobileNet with color augmentation, AdamW, label smoothing and cosine schedule over 30 epochs. 

2. Once again, the camera-like validation accuracy of each variant is shown in the table bellow

3. Here's the table:
| Change                                   | Reference                               | Δ val  | Δ camera-like | Larger than noise?                                             |
| ---------------------------------------- | --------------------------------------- | ------ | ------------- | -------------------------------------------------------------- |
| Geometric augmentation (scratch)         | Task 3 baseline (0.713 / 0.506)         | −0.069 | +0.011        | Val drop ~2 images, borderline; camera-like: no                |
| + colour jitter (scratch)                | Geometric aug (0.644 / 0.517)           | +0.011 | +0.035        | No                                                             |
| Pretrained, whole network fine-tuned     | Task 3 baseline                         | 0.000  | +0.103        | Camera-like: yes; val: no                                      |
| Pretrained, last layer only              | Fine-tuned (0.713 / 0.609)              | −0.012 | −0.046        | No                                                             |
| Pretrained + colour aug                  | Fine-tuned                              | +0.046 | +0.081        | Camera-like: yes; val: borderline (~1.5 images)                |
| + cosine scheduler, 30 epochs            | Pretrained + colour aug (0.759 / 0.690) | +0.034 | −0.023        | No                                                             |
| Dropout 0.3 (scratch)                    | Task 3 baseline                         | −0.058 | +0.023        | No (did not help)                                              |
| Global average pooling (scratch)         | Task 3 baseline                         | −0.173 | −0.138        | Yes, clearly worse                                             |
| AdamW, weight decay 0.05 (scratch)       | Task 3 baseline                         | 0.000  | −0.035        | No                                                             |
| Label smoothing 0.1 (scratch)            | Task 3 baseline                         | −0.023 | −0.035        | No                                                             |
| Combination of all (scratch)             | Task 3 baseline                         | −0.253 | −0.104        | Yes, clearly worse                                             |
| Combination (pretrained)                 | Pretrained fine-tuned                   | +0.069 | +0.115        | Camera-like: yes; val: borderline (~2 images)                  |
| Extra photos (scratch, 107 → 807 images) | Task 3 baseline                         | +0.046 | −0.058        | No on val/camera-like; leaves/val 0.347 → 0.808 (matched data) |

The table clearly shows how each variant either improved or lowered the validation accuracy, whether it is actually an effect of the variant or just noise. I would put the best combination (stated in point 1) on the phone. Unfortunately, I wasn't able to find the course camera app, so I am not attatching a screenshot.
What did not help: Dropout, AdamW, label smoothing and the cosine schedule (when alone) stayed within noise
    Geometric augmentatio lowered validation acc on the scratch network
    Global average pooling and the scratch combination were clearly worse, probably because the network is too small for them.