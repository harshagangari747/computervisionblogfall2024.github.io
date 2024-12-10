<h2 align="center"> <b></b>Solving 'SPOTTING THE DIFFERENCES' puzzle using Computer Vision </h2></p>
<p align="center"><i><b>Sai Harsha Gangari, &nbsp; Sairam Reddy Malreddy</b></i></p>

<h3>Introduction</h3>
<p>Spotting the differences in two similar images is a fun game to solve. But sometimes, this game can become more difficult and tricky to solve as an adult. We got an idea to solve this game using computer vision techniques and advancements that are made in the recent years. This game is all about spotting the objects which are modified in some or the other way. For example: change in color, missing object or misplaced object from one image etc. This lead us to chose a object detection model called <b>YOLO</b> which was released in the year 2020.</p>
<p> We designed a website that gives a user a pair of 'similar' images. The user needs to detect the objects that are different in both the images. If the user can't figure out any differences, we give a set of 'clues' to guide the user to spot the differences. The image and clue generation is made using computer vision models; YOLO and DALL-E</p>

<h3>Objective</h3>
<p>Given a pair of two similar images, the objective is to generate clues to guide player to spot the differences in both the images. The clues could be about the objects itself or position of the objects</p>

<h3>Approach</h3>
<p>Our approach includes three steps.</p>

<ol>
  <li><b>Image Generation using AI models.</b>
    <ul>
    <li>We need to generate a pair of images that have some differences between them.This task is done using Ai-image generators such as Dall-E (by Open AI), Imagen 3(by Google), Copilot (by Microsoft), 	Text2img (by DeepAI), AI Image Generator (by Canva)</li>
  </ul>
  </li>
  
  <li><b> Spotting differences in the two images</b>
    <ul>
    <li>We have subtracted the image from one another we generated the mask image that implies the subtraction of pixel values using OpenCV python package.</li>
  </ul>
  </li>
  
  <li><b>Generating the clues by identifying the objects that are difference mage.</b>
    <ul><li>
      After finding the pixel differences we get the contour regions as region of interest (ROI) in the difference mask image and then this region is given to the computer vision model YOLO to detect the object in them and found the relevant position of ROI in the image. 
      <li>Thus, we generated the clues from the objects that were detected.
The image is divided into nine(9) parts to be more detailed while giving clue. </li>
    </li></ul>
  </li>
</ol>


<h3>The Web Application</h3>
<p>We developed a web application to display the images and 'reveal clues' buttons. We used python Flask api for backend and ReactJs for frontend. The webpage contains two components. One is image and other is button</p>
<ul>
  <li>Player sees two similar images. Player needs to identify the areas in both the image where they differ</li>
  <li>There are two button 'Reveal Next Clue' and 'Reveal All Clues'.</li>
  <ul>
    <li><b>Reveal Next Clue: </b>Reveals the next clue available on clicking</li>
    <li><b>Reveal All Clues: </b>Reveals all the clues at once upon clicking</li>
  </ul>
</ul>

<h3>Architecture</h3>
<p> The following image briefly depicts the architecture of the program</p>
<div align="center">
<img width="50%" align="center" src="https://github.com/user-attachments/assets/c7ba3502-03e8-464b-9bd5-db4cdcc948e7" alt="Architecture Diagram" />
<br/>
<p><b>Figure 1: Architecture Diagram</b></p></div>

<h3>Object detection model selection</h3>
<p>We need a object detection model that could help us to identify the objects that are different in both the images. We agreed to use YOLO model. But there are many YOLO models.
We thought of trying to run our experiments on YOLOv8 model as it is SOTA in recent releases and also on YOLOv11n model which is a very recent release. We compared the results of both the object detection models</p>

<h3>Image division</h3>
We logically divide image to give 'positional clue' about the object for the player to look for. The regions in the image can be comprehended as below mentioned area
<table align='center'>
  <tr align='center'><td>top left</td><td>top</td><td>top right</td></tr>
  <tr align='center'><td>left</td><td>center</td><td>right</td></tr>
  <tr align='center'><td>bottom left</td><td>bottom</td><td>bottom right</td></tr>
</table>

<p>Now we shall dive into the above three steps in detail</p>

<!-- ===================================================================Task 1 Begin =================================================================== -->
<h2>Task1: Collecting the Images</h2>

<p>In our first task, we queried many of computer vision models that are available out there on internet. We queried DALL-E, Google Gemini, Microsoft's Copilot. We realized that since we are using the YOLO model, we thought of keeping the objects that are only detectable by the this model. So for the prompts that we gave, we tried to include a list of objets that we are particularly interest, which were also detectable by YOLO </p>


<h3>1. DALL-E: Image Generation tool based on the Prompt given, by OpenAI</h3>
<b>Prompt 1</b>
<pre>"Generate image pair that can be used in spotting differences between two image games such that the objects in image should contain 
Person, bird, cat, cow, dog, horse, sheep, aeroplane, bicycle, boat, bus, car, motorbike, train, bottle, chair, dining table, potted plant, sofa, tv/monitor."</pre>
<div >
  <p><p>Resulting image</p></p>
  <div align='center'>
    <img width="500px" height="200px" alt="image" src="https://github.com/user-attachments/assets/d2b89383-1d70-4d1b-a355-abfa16b22a5e">
  </div>
  <p>The above is generated when the prompt is given to DALL-E the image just left-shift of image and that does not have much noticeable difference in them (just a bottle on the table) </p>
</div>


<b>Prompt 2</b>
<pre>"Generate image pair that can be used in spotting differences between two image games such that the objects in image should contain 
Person, bird, cat, cow, dog, horse, sheep, aeroplane, bicycle, boat, bus, car, motorbike, train, bottle, chair, dining table, potted plant, sofa, tv/monitor. enerate simple images not much complicated. "</pre>
<div >
  <p><p>Resulting image</p></p>
  <div align='center'>
    <img width="400px" height="200px" alt="image" src="https://github.com/user-attachments/assets/ea4f7524-be89-4a70-af81-fc6f329da4e1">
  </div>
  <p>The above generated by the DALL-E but less complex with significant differences in them (like bottle on the table, dogs at the Bottom-Left and Bottom-Right and flowerpots at top-Left and Bottom-Right). Although this image is also a Left-Shift image of first one comparatively is better than Image-1.</p>
</div>

<b>Prompt 3</b>
<pre>"'Continuation of the chat...' -The above image is also a left shift of the first image I need perfectly aligned and specific differences in the images. "</pre>
<div >
  <p><p>Resulting image</p></p>
  <div align='center'>
    <img width="400px" height="200px" alt="image" src="https://github.com/user-attachments/assets/5f2a77fa-cfe6-4612-839f-1c1537ea67df">
  </div>
  <p>The above image is generated by DALL-E which is having difference between two scenes and a very less shift compared to earlier scenes.</p>
</div>

<p>At this point we thought to generate images by giving the real image examples along with the yolo detectable objects to check if we could make DALL-E to generate accurate images</p>
<b>Prompt 4 </b>
<pre>Create an image of a scenery/landscape by taking the reference of above image. 
Use only objects from this list 0: 'person', 1: 'bicycle', 2: 'car', 3: 'motorcycle', 4: 'airplane', 5: 'bus', 6: 'train', 7: 'truck', 8: 'boat', 9: 'traffic light', 10: 'fire hydrant', 11: 'stop sign', 12: 'parking meter', 13: 'bench', 14: 'bird', 15: 'cat', 16: 'dog', 17: 'horse', 18: 'sheep', 19: 'cow', 20: 'elephant', 21: 'bear', 22: 'zebra', 23: 'giraffe', 24: 'backpack', 25: 'umbrella', 26: 'handbag', 27: 'tie', 28: 'suitcase', 29: 'frisbee', 30: 'skis', 31: 'snowboard', 32: 'sports ball', 33: 'kite', 34: 'baseball bat', 35: 'baseball glove', 36: 'skateboard', 37: 'surfboard', 38: 'tennis racket', 39: 'bottle', 40: 'wine glass', 41: 'cup', 42: 'fork', 43: 'knife', 44: 'spoon', 45: 'bowl', 46: 'banana', 47: 'apple', 48: 'sandwich', 49: 'orange', 50: 'broccoli', 51: 'carrot', 52: 'hot dog', 53: 'pizza', 54: 'donut', 55: 'cake', 56: 'chair', 57: 'couch', 58: 'potted plant', 59: 'bed', 60: 'dining table', 61: 'toilet', 62: 'tv', 63: 'laptop', 64: 'mouse', 65: 'remote', 66: 'keyboard', 67: 'cell phone', 68: 'microwave', 69: 'oven', 70: 'toaster', 71: 'sink', 72: 'refrigerator', 73: 'book', 74: 'clock', 75: 'vase', 76: 'scissors', 77: 'teddy bear', 78: 'hair drier', 79: 'toothbrush'.
Two image should be similar and copy of each other. But make changes to few objects for example, change the position of an object, change color of an object, delete an object, add a new object from the list. When I overlap the images, there should not be any pixel shift of the images. There should exactly align most of the static objects when overlapped.
</pre>
<div >
  <p>Resulting image</p>
  <div align='center'>
    <img width= '50%' src = 'https://github.com/user-attachments/assets/c75c91d9-cc21-42ee-89e7-9df948413b7a'/>
  </div>
  <p>Image is generated by DALL-E where we have given more specific description to generate image containing of specific objects in image. This image also has slight shift from one another (almost neglectable). We have also generated lot more images with different prompts, but image that are most accurately generated from the prompts are discussed here.</p>
</div>


<h3>2. Copilot: Image Generation Tool based on prompt given, by Microsoft.</h3>
<b>Prompt 3</b>
<pre>"Generate image pair that can be used in spotting differences between two image games such that the objects in image should contain 
Person, bird, cat, cow, dog, horse, sheep, aeroplane, bicycle, boat, bus, car, motorbike, train, bottle, chair, dining table, potted plant, sofa, tv/monitor."
![image](https://github.com/user-attachments/assets/a13a71bf-af44-44a7-be89-589823170dce)
</pre>
<div >
  <p><p>Resulting image</p></p>
  <div align='center'>
    <img width="400px" height="200px" alt="image" src="https://github.com/user-attachments/assets/5f2a77fa-cfe6-4612-839f-1c1537ea67df">
  </div>
  <p>The above image is generated by DALL-E which is having difference between two scenes and a very less shift compared to earlier scenes.</p>
</div>


<!-- ===================================================================Task 2 Begin =================================================================== -->
<h2>Task 2: Finding the difference Image</h2>
<p>Difference image is key for us as we draw the boundaries/contours of the parts of both the images to get the sub region in the image which we feed to the YOLO model for object detection. We used python's <b>opencv-python</b> package to find the difference mask as well as the contours in the images. We used the `absdiff` method and  `findContours` method to obtain the difference images as well as the contours from both the images respectively.  </p>
<p>Here are some difference images that the program produced. These are the images that are produced using DALL-E</p>
<table>
  <tr height="50%">
    <td><img height=350px width=300px src='https://github.com/user-attachments/assets/07879f06-d635-415a-a66f-1fa87ed3e571'/></td>
    <td><img height=350px width=300px src="https://github.com/user-attachments/assets/cbcc23c9-95e1-473b-a34e-12c7bddc5c4c"/></td>
    <td><img height=350px width=300px src="https://github.com/user-attachments/assets/181e6c91-527c-4130-8cca-a6dcae84d099"/></td>
  </tr>
</table>
<p> You may ask a question that why does difference image look 'odd' and is not what one expect since this image has clearly defined outlines of the objects in both the images when it should not. This is because, DALL-E tried to give similar images but second image is the scene shift from the first image. This means that all the objects in the first image were shifted towards left or right in the second image. We think this happend because, the prompt that we gave to DALL-E to give two 'similar' images, DALL-E might thought that since these two images are shifted, there are significant changes in both the images yet preserving the alignment of the corresponding objects at their respective position in both the images.</p>
<!-- ===================================================================Task 3 Begin =================================================================== -->
<hr></hr>
<h2>Task 3: Generating the clues by identifying the objects that are in difference image</h2>
<p>Now our task is to get the sub regions of both the image at given contours. Contours are extracted from the difference image that we generated in above step. For each of the sub region from both the images, we would like to detect if there are any objects in the said space. If yes, we store the results for further clue generation.</p>
<br/>
<p>The object detection on sub regions of both the images are run as experiments on both YOLOv8m and YOLOv11n models. We now discuss the experiments first on YOLOv8m model and then followed by YOLOv11n model. For all the experiments we considered only the images that were generated by the DALL-E</p>

<h3>Expectation from the model</h3>
<p>As the images generated by the DALL-E model are not accurate or atleast what we expected and since it is 'AI' generated, we expect the model to detect those objects which are clearly and easily found just by seeing the image.</p>

<!-- ===================================================================Experiment on YOLOv8 Begin =================================================================== -->

<h3>Experiments on YOLOv8m model</h3>
<hr width='25%vw'></hr>
<h4>Experiment 1</h4>
<div>
  <div>
    <p><h5>Input image</h5></p>
    <div align='center'><img width= '50%' src = 'https://github.com/user-attachments/assets/c75c91d9-cc21-42ee-89e7-9df948413b7a'/></div>
    <p>As we can see the fire hydrant, bicycle, traffic signal and hot-air ballon are clearly and easily distinguishable, we expect the model to atleast detect the same</p>
  </div>
  <div>
    <p><h5>The output</h5></p>
    <div><img width="1512" alt="yolo8m-cactus" src="https://github.com/user-attachments/assets/457e60dd-4186-4292-91ed-63e581c2be9c"></div>
    <b>Analysis</b>
    <p>We see that the clues are generated by the program. If we closely look at the clues, we got three of our major clues that were alredy detected by the model: fire hydrant, bicycle, traffic light. The hot-air balloon got unrecognized and we believe this had happend because the model itself was not trained on detecting the hot-air balloons. Additionally we also got a clue that 'bird from top left' is different in images which is indeed different as in image1 the bird is half visible and in image2 the same bird in visible clearly</p>
    <br/>
    <p>There are few mispredictions made by the model in which manier times bird was detected. This might have happend as the blades of the grass near the cactus in the bottom left corner might be percieved as the wing of the bird. The clue about 'car' and 'bench' was purely because of the scene shift issue of the image. </p>
  </div>
</div>


<h4>Experiment 2</h4>
<div>
  <div>
    <p><h5>Input image</h5></p>
    <div align='center'><img width='50%' src='https://github.com/user-attachments/assets/40d4a648-def7-4f20-b78c-3f6d0a6bc91e'/></div>
    <p>Here we are expecting the model to detect the dog, plant pot inplace of dog in image 1 on bottom right and the brown plant plot towards the right. Let's see how the model detects</p>
  </div>
  <div>
    <p><h5>The output</h5></p>
    <div><img width="1512" alt="yolo8m-dogman" src="https://github.com/user-attachments/assets/643bbea6-b0a3-4cd9-9ae3-039e2b172036"></div>
</div>
    <b>Analysis</b>
    <p>After taking a closer look at the clues, the model detected the dog and brown flower pot at the bottom right and the flower pot on the top left correctly</p>
    <br/>
    <p>Additional clues include about the bottle at center. This clue is correct as the green bottle in the center of the grid and on the table is differing terms of height</p>
  <p>If we closely look at clues, one of the clue is about the 'wine glass' that was holding by the man in the image. It is surprising that the model was able to detect the objects whose outline was not sharp and pixellated. The clues about the dining table, umbrella, person are because of pixel shifts ( outlines detected in the difference mask image)</p>
  <p>It is some what surprising that the flower vase on the top left is completely missed out by the model</p>
  </div>
</div>

<h4>Experiment 3</h4>
<div>
  <div>
    <p><h5>Input image</h5></p>
    <div align='center'><img width= '50%' src='https://github.com/user-attachments/assets/6c5b1bb7-f9f3-46cd-80e8-7234012fbf9c'/> </div>
 </div>
    <p>Here we are expecting the model to detect the dog at the bottom</p>
  </div>
  <div>
    <p><h5>The output</h5></p>
    <div><img width="1512"  alt="yolo8m-garden" src="https://github.com/user-attachments/assets/8cc002f3-96b7-4c9a-b334-716b9e10b393"></div>
</div>
    <b>Analysis</b>
    <p>The model tried to figure out the dog but instead detected it as a teddy bear. We believe that this happend because the outline of the dog was not well defined and is not a real image, and main because the teddy bear has the snout like a dog, tail like a dog and face like a dog, the model was confused between a dog and a teddy bear </p>
    <br/>
  </div>
</div>

<!-- ===================================================================Experiment on YOLOv11 Begin =================================================================== -->

<h3>Experiments on YOLOv11n model</h3>
<hr width='25%vw'></hr>
<h4>Experiment 1</h4>
<div>
  <div>
    <p><h5>Input image</h5></p>
    <div align='center'><img width= '50%' src = 'https://github.com/user-attachments/assets/c75c91d9-cc21-42ee-89e7-9df948413b7a'/></div>
    <p>Again, we expect the model to atleast detect a fire hydrant, traffic light, hot-air balloon, bicycle </p>
  </div>
  <div>
    <p><h5>The output</h5></p>
    <div align='center'> <img width="1512" alt="yolo11-cactus" src="https://github.com/user-attachments/assets/6be76797-e53c-4705-ba40-5f4730272915">
 </div>
    <b>Analysis</b>
    <p>This time, on yolo11n, the model detected many objects. If we look at the clues, the model detected bicycle, fire hydrant correctly but leaving out the hot-air balloon and traffic light. What interesting in this experiment is that the model is treating the 'long head' of the cactus as the surfboard. We are convinced that the shape of surfboard can be a possible for the given shape of the cactus. Other interesting detection is that 'banana' at the center of the image. We feel this detection was a result of the model mispredicting the stem of the larger cactus as banana. Again, we were convinced by looking at the shape of the stem as it has slight curve resembling the raw green banana</p>
    <br/>
    <p>There are many mispredictions made by the model. The model struggled to recognize the 'not so well' defined outlines of the objects. All other mispredictions are due to shift in the scenes from one image to another</p>
  </div>
</div>


<h4>Experiment 2</h4>
<div>
  <div>
    <p><h5>Input image</h5></p>
    <div align='center'><img width='50%' src='https://github.com/user-attachments/assets/40d4a648-def7-4f20-b78c-3f6d0a6bc91e'/></div>
    <p>As from the experiment 2 using yolov8m , we are expecting the model to detect the dog, plant pot inplace of dog in image 1 on bottom right and the brown plant plot towards the right. Let's see how the model detects</p>
  </div>
  <div>
    <p><h5>The output</h5></p>
    <div align='center'> <img width="1512" src="https://github.com/user-attachments/assets/7f46d97e-aac5-4a32-8b52-1638ec2e08eb">
  </div>
</div>
    <b>Analysis</b>
    <p>Analysing the clues, we could infer that the expected objects are detected by the model at said places. E</p>
    <br/>
    <p>Additional clues include about the bottle at center.Again, this clue is correct as the green bottle in the center of the grid and on the table is differing terms of height</p>
  <p>What is surprising is that the dog in bottom right is treated as a teddy bear by the model. As explained earlier, this could because of the similarities of teddy bear and dog and the dog body is not fully visible hence directing model to predict dog as teddy bear</p>
  </div>
</div>

<h4>Experiment 3</h4>
<div>
  <div>
    <p><h5>Input image</h5></p>
    <div align='center'><img width= '50%' src='https://github.com/user-attachments/assets/6c5b1bb7-f9f3-46cd-80e8-7234012fbf9c'/> </div>
 </div>
    <p>Here we are expecting the model to detect the dog at the bottom</p>
  </div>
  <div>
    <p><h5>The output</h5></p>
    <div align='center'> <img width="1512" alt="yolo11-garden" src="https://github.com/user-attachments/assets/b9b0467d-de58-482b-a3e0-5596b1854dd6">
</div>
</div>
    <b>Analysis</b>
    <p>This time, this model completely mispredicted the objects. Only one correct clue could be generated which a potted plant on the top right corner. The model completely missed the dog in both the images. We are unclear why model missed the dog this time</p>
    <br/>
  </div>
</div>












