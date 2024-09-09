#### Analysing Incorrect Responses of Microsoft Copilot and Google Gemini on a given image

## Introduction:
The purpose of this blog is to analyze the incorrect responses given by Microsoft Copilot and Google Gemini Large Language Models when prompted with questions about an image.
The image that we chose is
![image](https://github.com/user-attachments/assets/a6c9e41c-f803-43e3-83ff-f3a6f5458ce7)
We have provided the above image to both google Gemini and Microsoft copilot and we asked few questions about the image. Though we got accurate answers, there were few incorrect answers given by both LLMs.

**CoPilot:**

Prompt 1: What is this image about? Copilot: This image shows three cows in a peaceful pasture. Two cows are prominently in the foreground, while one is partially visible in the background. They are brown with white patches, likely a dairy breed, and are standing among birch trees with distinctive white bark. The scene suggests a rural or agricultural setting, highlighting livestock farming or the natural environment where these cows graze. Our Hypothesis: The image contains only two cows, but the copilot mistakenly interpreted the thurl of one cow as another cow, since that part closely resembled the cow's face.

Prompt 2: How many trees are there in this image? Copilot: In the image, there are three birch trees visible. They have distinctive white bark with black markings, adding to the serene rural setting. Our Hypothesis: The model was able to clearly distinguish the trees from other objects in the image, demonstrating its ability in scenarios where the objects are clear and in the foreground.

Prompt 3: How many eyes are visible in this image? Copilot: In the image, there are four eyes visible. Each of the two cows in the foreground has both eyes visible, while the cow in the background does not have any visible eyes. Our Hypothesis: The model might have assumed that both eyes of each cow were visible based on a prior expectation of symmetry, even though only one eye was visible on each cow.

**Bard:**


