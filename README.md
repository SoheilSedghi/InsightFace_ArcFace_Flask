# **server:**

1. Model Initialization:

  Loads and initializes pre-trained models (buffalo_s, buffalo_l) for subsequent client requests.
  the shape of inputs set to (480*480) as most of users do. The threshould of face recognition is set to 0.5 as default. It was better to change this value because some images in CPLFW cannot be recognized by the model. The treshould value for pair recognition is set to 0.24.

2. Flask Server Setup:

  Deploys a Flask server on a Colab thread to handle HTTP POST requests.

3. Request Handling:

  Upon receiving a client request:

  * decode two images using base64 and gets selected model from the request payload.
  * Computes embeddings for both images using the specified pre-trained model.
  * Calculates the cosine similarity between the computed embeddings.
  
  **Cosine Similarity(A, B) = (A · B) / (||A|| * ||B||)**

  * Compares the calculated similarity to a predefined threshold.
  * Returns a response indicating a match (True) if the similarity exceeds the threshold, or a mismatch (False) otherwise. The response is in JSON format.

# **client:**

1. download Datasets (LFW, CALFW and CPLFW) and extracting pairs from specific pair file.
2. load each pair, encode using base64 and send as a http POST request containing name of selected model in JSON format.
3. get response from server and store in a variable.
4. calculate accuracy.

**Accuracy = (TP + TN) / (TP + TN + FP + FN)**

Where:

TP (True Positive): The system correctly identifies two images as belonging to the same person.

TN (True Negative): The system correctly identifies two images as belonging to different people.

FP (False Positive): The system incorrectly identifies two images of different people as belonging to the same person.

FN (False Negative): The system incorrectly identifies two images of the same person as belonging to different people.


FMR (False Match Rate)

FMR measures the likelihood that the system will incorrectly identify two images of different people as belonging to the same person. It's the ratio of false positives to the total number of impostor pairs.

Formula:

**FMR = FP / (FP + TN)**

FNMR (False Non-Match Rate)

FNMR measures the likelihood that the system will incorrectly identify two images of the same person as belonging to different people. It's the ratio of false negatives to the total number of same pairs.

Formula:

**FNMR = FN / (FN + TP)**
