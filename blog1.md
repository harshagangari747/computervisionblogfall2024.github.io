# From Precision to Perplexity: Assessing AI's Image Interpretation Skills</br>

### <ins>Introduction:</ins></br>
The purpose of this blog is to analyze the responses given by Microsoft Copilot and Google Gemini Large Language Models when prompted with questions about an image. We ask questions to get the LLM a 'wrong answer' and then hypothesize for why the chosen LLM gave the wrong answer.
We chose the following image.</br>
![image](https://github.com/user-attachments/assets/a6c9e41c-f803-43e3-83ff-f3a6f5458ce7)
</br>
<!--We have provided the above image to both google Gemini and Microsoft copilot and we asked few questions about the image. Though we got accurate answers, there were few incorrect answers given by both LLMs.</br>-->
### <ins>CoPilot:</ins>
</br></br>
<b>Prompt 1:</b> What is this image about?</br> <b>Copilot:</b> This image shows three cows in a peaceful pasture. Two cows are prominently in the foreground, while one is partially visible in the background. They are brown with white patches, likely a dairy breed, and are standing among birch trees with distinctive white bark. The scene suggests a rural or agricultural setting, highlighting livestock farming or the natural environment where these cows graze. </br>
<b>Our Thoughts:</b> The image contains only two cows, but the copilot mistakenly interpreted the thurl of one cow as another cow, since that part closely resembled the cow's face.</br></br>

<b>Prompt 2:</b> How many trunks are visible near the cows?</br> <b>Copilot:</b> There are three trunks visible near the cows in the image you provided. The scene looks quite peaceful with the cows grazing in a sunny, green pasture.</br> <b>Our Thoughts:</b> The model was able to clearly distinguish the trees from other objects in the image, demonstrating its ability in scenarios where the objects are clear and in the foreground.</br></br>

<b>Prompt 3:</b> How many eyes are visible in this image?</br> <b>Copilot:</b> In the image, there are four eyes visible. Each of the two cows in the foreground has both eyes visible, while the cow in the background does not have any visible eyes.</br> <b>Our Thoughts:</b> The model might have assumed that both eyes of each cow were visible based on a prior expectation of symmetry, even though only one eye was visible on each cow.</br></br>

<b>Prompt 4:</b> Is there any fence in the background?</br>
<b>Copilot:</b> There is no fence visible in the background of the image.</br>
<b>Our Thoughts:</b> Although the fence is not clearly visible, it is present in the background, indicating that the model struggles to analyze objects that aren't clearly visible.</br></br>

<b>Prompt 5:</b> Is there any house in the image? If yes, what is the color of the house?</br>
<b>Copilot:</b> There is no house present in the image.</br>
<b>Our Thoughts:</b> There are houses in the background, though they are blurry, indicating that the model struggles to analyze objects that aren't clearly visible.</br>
</br></br>
### <ins>Gemini:</ins>
</br></br>
<b>Prompt 1:</b> what is this image about?</br>
<b>Gemini:</b> This image features two cows standing in a grassy field next to a grove of birch trees. The cows are brown with white patches, and they appear to be grazing. The background is a lush green landscape with a clear blue sky.</br>
<b>Our Thoughts:</b> The model accurately analyzed the image and provided concise information.Unlike copilot, gemini was able to correctly answer this question, demonstrating its capability to distinguish between similar-looking objects.</br></br>

<b>Prompt 2:</b> How many eyes are visible in this image?</br>
<b>Gemini:</b> Two eyes are visible in the image. You can see the eyes of one cow looking directly at the camera and one eye of the other cow looking off to the side.</br>
<b>Our Thoughts:</b>  Gemini was able to correctly answer this question, showing that it is gathering information from the image without making any inferences based on the data it was trained on.</br></br>

<b>Prompt 3:</b> How many trunks are visible near the cows?</br>
<b>Gemini:</b> Two trunks are visible near the cows. They belong to the two birch trees standing on either side of them.</br>
<b>Our Thoughts:</b> The model answered this question incorrectly, possibly because it failed to recognize the division between the two trees. The image is a bit confusing, as it resembles a single trunk that then splits into two.</br></br>

<b>Prompt 4:</b> Is there any fence in the background?</br>
<b>Gemini:</b> Yes, there appears to be a fence in the background of the image. It's partially obscured by the trees, but we can see a portion of it.</br>
<b>Our Thoughts:</b> The model successfully identified the fence in the background, demonstrating its ability to detect and analyze objects even when they are not clearly visible.</br></br>

<b>Prompt 5:</b> Is there any house in the image? If yes, what is the color of the house?</br>
<b>Gemini:</b> Based on the provided image, there is no house visible. The image primarily depicts a natural landscape with trees and cows. There are no structures or buildings apparent in the background.</br>
<b>Our Thoughts:</b> There are houses in the background, though they are blurry, indicating that the model struggles to analyze objects that aren't clearly visible.</br></br>

### <b> <ins>Final Thoughts:</ins></b></br> 
Based on our analysis of the given image, Gemini clearly outperforms Copilot. This discrepancy may be attributed to the fact that the domain we are testing might not align with the primary focus for which these models were developed. Gemini, designed to handle a broader range of AI tasks, was able to address our questions efficiently due to its extensive training on diverse datasets. In contrast, Copilot, which is specifically developed to assist developers with coding tasks and is relatively new compared to Gemini, may not have been extensively trained on datasets relevant to image analysis, which could explain its less effective performance in this scenario.</br>






