# Activity Recognition for Autism Detection 

Code for **Activity Recognition for Autism Detection**.

**[Authors](mailto:anish.lakkapragada@gmail.com,peter100@stanford.edu,dpwall@stanford.edu)**: [Anish Lakkapragada](mailto:anish.lakkapragada@gmail.com), [Peter Washington](mailto:peter100@stanford.edu), [Dennis P. Wall](mailto:dpwall@stanford.edu)

<details>
    <summary>Abstract</summary>
     insert final abstract here 
</details>

## Objective 

Today's autism diagnosis is a quite lengthy and inefficient process. Families often have to wait a few years before receiving a diagnosis, and this problem is exacerbated by the fact that the earliest possible intervention is required for best clinical outcomes. One of the biggest indicators of autism is self-stimulatory, or stimming, behaviors such as hand flapping, headbanging, and spinning. In this paper, we demonstrate successful lightweight detection of hand flapping in videos using deep learning and activity recognition. We believe such methods will help create a remote, fast, and accessible autism diagnosis. 

## Data 

In order to collect the videos, we use the [Self-Stimulatory Behavior Dataset (SSBD)](https://rolandgoecke.net/research/datasets/ssbd/) which contains 75 videos of hand flapping, head banging, and spinning behavior. We extract 100 hand flapping and control 2-3 second videos from this dataset and use it 
to train our model. Because most of the videos in SSBD are children, we wanted to see whether a model we built could generalize past age (and hand shape), so we recorded 3 positive and 3 control videos of ourselves. 

Our entire dataset can be found in a google drive folder [here](https://tinyurl.com/47fya6).

## Approach 

In this space, one way to detect hand flapping in videos that has been done before is to use pose estimation images and feed them into time-series based 
Recurrent Neural Networks (RNNs). However because these approaches use heavier feature representations, they demand heavier models; this is not ideal for creating models to deploy into mobile phones, etc. We decide to use a much lighter feature representation and model by using landmark detection to find the
coordinates of the detected hand landmarks as a feature representation over time which is then fed into a Long-Short Term Memory (LSTM) network. 

<img src = "docs/mediapipe_landmarks.png">

The above image shows the 21 landmarks that Mediapipe, a landmark detection framework made by Google, recognizes on the hands. We take the first 90 frames of a video, and at each frame,  take the detected <em> (x, y, z) </em> coordinates of a set amount of landmarks and concatenate them to become a "location frame". Note that the location frame is always 6x bigger than the set amount of landmarks, because there are 3 coordinates for a given landmark and 2 hands where that landmark can be found. If a landmark is not detected, we set all of its <em> (x, y, z) </em> coordinates to 0. This location vector for every frame is fed into an LSTM network that then gives us our prediction. We train models with and without augmentation and compare their results. Our overall approach is shown in the below image. 

<img src = "docs/Approach.png">

It is easy to see that this kind of feature representation and model architecture would easily fit into a model device even unoptimized. Our heaviest model uses <50,000 parameters. 

## Approaches

## Results

We found that the best results on our recorded videos and on our dataset came from using the six landmarks approach. This approach almost always predicted correctly on our self-recorded videos and got a good, consistent accuracy on our dataset. It's (validation) accuracy, precision, recall, F1 Score, and AUROC over 10 runs are shown below.

| Accuracy | Precision   | Recall      | F1 Score    | AUROC       |
|-------------------------|-------------|-------------|-------------|-------------|
| 71.9 ± 1.7              | 70.8 ± 1.85 | 74.5 ± 4.04 | 71.9 ± 2.25 | 0.77 ± 0.03 |

<details>
    <summary> All Results </summary>
    <table>
<thead>
<tr>
<th>Approach</th>
<th>Classification Accuracy</th>
<th>Precision</th>
<th>Recall</th>
<th>F1 Score</th>
<th>AUROC</th>
<th>Video Performance</th>
</tr>
</thead>
<tbody>
<tr>
<td>All Landmarks</td>
<td>72.4 ± 0.8</td>
<td>69.68 ± 0.99</td>
<td>82.92 ± 0.94</td>
<td>75.15 ± 0.57</td>
<td>0.75 ± 0.02</td>
<td>🤮</td>
</tr>
<tr>
<td>Mean Landmark</td>
<td>69.8 ± 4.04</td>
<td>69.18 ± 5.05</td>
<td>69.78 ± 6.56</td>
<td>67.86 ± 3.52</td>
<td>0.75 ± 0.02</td>
<td>😐</td>
</tr>
<tr>
<td>One Landmark</td>
<td>73.9 ± 2.77</td>
<td>75.29 ± 1.72</td>
<td>73.1 ± 5.09</td>
<td>72.6 ± 2.30</td>
<td>0.77 ± 0.02</td>
<td>👍</td>
</tr>
<tr>
<td>Six Landmark</td>
<td>71.9 ± 1.7</td>
<td>70.8 ± 1.85</td>
<td>74.5 ± 4.04</td>
<td>71.9 ± 2.25</td>
<td>0.77 ± 0.03</td>
<td>🔥</td>
</tr>
</tbody>
</table>
</details>
## Code 


## Citation 

## Acknowledgements 