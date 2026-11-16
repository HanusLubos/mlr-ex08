#REVIEW - Task 2 Leaf classifier on your device
Hanus Luboš, 256983
--------------------
1. ![Screenshot from classifier on mobile phone](image.png)
As seen on screenshot, the classifier does open from the URL, but doesn't classify the image correctly. The web app was open on mobile device, but the photo was open on a PC screen.

2. The classifier used on the .jpg file does show the same probabilities to three decimals as the .json file. However, the classifier is trained on untrue data (wrongly labeled), meaning the classification is very often wrong.

3. When checking the sourcecode of the html site, the order of the chanels going into the tensor matches the one in model_info.json (line 117 of the html) and the transform of the notebook

4. I didn't find any sign of the image being sent anywhere. The only URL besides the github.io I found is this one: https://cdn.jsdelivr.net/npm/onnxruntime-web@1.31.1/dist/ort.min.js