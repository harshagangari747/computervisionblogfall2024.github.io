<h2 align="center"> <b></b>Solving 'SPOTTING THE DIFFERENCES' puzzle using Computer Vision </h2></p>
<p align="center"><i><b>Sai Harsha Gangari, &nbsp; Sairam Reddy Malreddy</b></i></p>

<h3>Introduction</h3>
<p>Spotting the differences in two similar images is a fun game to solve. But sometimes, this game can become more difficult and tricky to solve as an adult. We got an idea to solve this game using computer vision techniques and advancements that are made in the recent years. This game is all about spotting the objects which are modified in some or the other way. For example: change in color, missing object or misplaced object from one image etc. This lead us to chose a object detection model called <b>YOLO</b> which was released in the year 2020.</p>
<p> We designed a website that gives a user a pair of 'similar' images. The user needs to detect the objects that are different in both the images. If the user can't figure out any differences, we give a set of 'clues' to guide the user to spot the differences. The image and clue generation is made using computer vision models; YOLO and AI Image Generation Tools</p>

<h3>Objective</h3>
<p>Given a pair of two similar images, the objective is to generate clues to guide player to spot the differences in both the images. The clues could be about the objects itself or position of the objects</p>

<h3>Approach</h3>
<p>Our approach includes three steps.</p>

<ol>
  <li><b>Image Generation using AI models.</b>
    <ul>
    <li>We need to generate a pair of images that have some differences between them.This task is done using Ai-image generators such as Dall-E (by Open AI), Imagen 3(by Google), Copilot (by Microsoft), 	AI Image Generator(also text2img by DeepAI), AI Image Generator (by Canva)</li>
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
<p>We need a object detection model that could help us to identify the objects that are different in both the images. We agreed to use YOLO model. But there are many versions of YOLO models. We thought of trying to run our experiments on YOLOv8 model as it is SOTA in recent releases and also on YOLOv11n model which is a very recent release. We compared the results of both the object detection models</p>

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

<p>In our first task, we queried many of computer vision models for AI Image Generation that are available out there on internet. We queried tools like DALL-E, Imagen 3 by Google Gemini, Microsoft's Copilot. We realized that since we are using the YOLO model, we thought of keeping the objects that are only detectable by the this model. So for the prompts that we gave, we tried to include a list of objets that we are particularly interest, which were also detectable by YOLO </p>


<h3>1. DALL-E: Image Generation tool based on the Prompt given, by OpenAI</h3>
<b>Prompt 1</b>
<pre>"Generate an image pair that can be used in "spotting differences between two images" games
such that the image should contain only objects like a person, bird, cat, cow, dog, horse,
sheep, aeroplane, bicycle, boat, bus, car, motorbike, train, bottle, chair, dining table,
potted plant, sofa, and TV/monitor."</pre>
<div >
  <p><p>Resulting image</p></p>
  <div align='center'>
    <img width="600" height="300" alt="image" src="https://github.com/user-attachments/assets/d2b89383-1d70-4d1b-a355-abfa16b22a5e">
  </div>
  <p><b>Image-1:</b> The above image is generated when the prompt is given to DALL-E the image on right is just left-shift of image on left and that does not have much noticeable difference in them (just a bottle on the table) </p>
</div>


<b>Prompt 2</b>
<pre>"Generate an image pair that can be used in "spotting differences between two images" games 
such that the image should contain only objects like a person, bird, cat, cow, dog, horse, sheep,
aeroplane, bicycle, boat, bus, car, motorbike, train, bottle, chair, dining table, potted plant,
sofa, and TV/monitor.
Generate a simple pair may not need to be complicated one"</pre>
<div >
  <p><p>Resulting image</p></p>
  <div align='center'>
    <img width="600" height="300" alt="image" src="https://github.com/user-attachments/assets/ea4f7524-be89-4a70-af81-fc6f329da4e1">
  </div>
  <p><b>Image-2:</b> The above generated by the DALL-E but less complex with significant differences in them (like bottle on the table, dogs at the Bottom-Left and Bottom-Right and flowerpots at top-Left and Bottom-Right). Although this image is also a Left-Shift image of image on rightside but comparatively better than Image-1.</p>
</div>

<b>Prompt 3</b>
<pre>"'Continuation of the chat...' -The above image is also a left shift of the first image 
I need perfectly aligned and specific differences in the images. "</pre>
<div >
  <p><p>Resulting image</p></p>
  <div align='center'>
    <img width="600" height="300" alt="image" src="https://github.com/user-attachments/assets/5f2a77fa-cfe6-4612-839f-1c1537ea67df">
  </div>
  <p><b>Image-3:</b> The above image is generated by DALL-E which is having soe differences in between two scenes and a very less shiftrd image pair compared to earlier scenes.</p>
</div>

<p>At this point we thought to generate images by giving the real image reference along with the yolo detectable objects to check if we could make DALL-E to generate accurate images</p>
<p>The below image was used as a reference to give to DALL-E</p>
<div align='center'>
  <img width='600' src='https://github.com/user-attachments/assets/f0c0580b-8bcb-4083-af02-4c81dd29377e'>

</div>
<b>Prompt 4 </b>
<pre>Create an image of a scenery/landscape by taking the reference of above image. 
Use only objects from this list 0: 'person', 1: 'bicycle', 2: 'car', 3: 'motorcycle', 4: 'airplane', 5: 'bus', 6: 'train', 7: 'truck', 8: 'boat', 9: 'traffic light', 10: 'fire hydrant', 11: 'stop sign', 12: 'parking meter', 13: 'bench', 14: 'bird', 15: 'cat', 16: 'dog', 17: 'horse', 18: 'sheep', 19: 'cow', 20: 'elephant', 21: 'bear', 22: 'zebra', 23: 'giraffe', 24: 'backpack', 25: 'umbrella', 26: 'handbag', 27: 'tie', 28: 'suitcase', 29: 'frisbee', 30: 'skis', 31: 'snowboard', 32: 'sports ball', 33: 'kite', 34: 'baseball bat', 35: 'baseball glove', 36: 'skateboard', 37: 'surfboard', 38: 'tennis racket', 39: 'bottle', 40: 'wine glass', 41: 'cup', 42: 'fork', 43: 'knife', 44: 'spoon', 45: 'bowl', 46: 'banana', 47: 'apple', 48: 'sandwich', 49: 'orange', 50: 'broccoli', 51: 'carrot', 52: 'hot dog', 53: 'pizza', 54: 'donut', 55: 'cake', 56: 'chair', 57: 'couch', 58: 'potted plant', 59: 'bed', 60: 'dining table', 61: 'toilet', 62: 'tv', 63: 'laptop', 64: 'mouse', 65: 'remote', 66: 'keyboard', 67: 'cell phone', 68: 'microwave', 69: 'oven', 70: 'toaster', 71: 'sink', 72: 'refrigerator', 73: 'book', 74: 'clock', 75: 'vase', 76: 'scissors', 77: 'teddy bear', 78: 'hair drier', 79: 'toothbrush'.
Two image should be similar and copy of each other. But make changes to few objects for example, 
change the position of an object, change color of an object, delete an object, 
add a new object from the list.When I overlap the images, there should not be 
any pixel shift of the images.There should exactly align most of the static objects when overlapped.
</pre>
<div >
  <p>Resulting image</p>
  <div align='center'>
    <img width= '600' height='300' src = 'https://github.com/user-attachments/assets/c75c91d9-cc21-42ee-89e7-9df948413b7a'/>
  </div>
  <p><b>Image-4:</b> Image is generated by DALL-E where we have given more specific description to generate image containing of specific objects in image. This image also has slight shift from one another (almost neglectable).</p>
  <p>We have also generated lot more images with different prompts, but image that are most accurately generated from the prompts are discussed here.</p>

</div>


<h3>2. Copilot: Image Generation Tool based on prompt given, by Microsoft.</h3>
<b>Prompt 1</b>
<pre>"Generate an image pair that can be used in "spotting differences between two images" games
such that the image should contain only objects like a person, bird, cat, cow, dog, horse, sheep,
aeroplane, bicycle, boat, bus, car, motorbike, train, bottle, chair, dining table, potted plant, 
sofa, and TV/monitor."
</pre>
<div >
  <p><p>Resulting image</p></p>
  <div align='center'>
    <img width="600" height='500' alt="image" src="https://github.com/user-attachments/assets/edc88edd-efbd-4be9-8a26-17b54e01bb10">
  </div>
  <p><b>Image-5:</b> This image is generated by copilot where the image left and right part of same image but the same scene which have difference. </p>
</div>

<b>Prompt 2</b>
<pre>"'Continuation of above chat...' -The above image is not used in spotting difference game the image 
should be of same scene with some differences in them."
</pre>
<div >
  <p><p>Resulting image</p></p>
  <div align='center'>
    <img width="600" height='500' alt="image" src="https://github.com/user-attachments/assets/02503688-2f54-44b8-b3b9-6068cb05b158">
  </div>
  <p><b>Image-6:</b> The above is generated by copilot where the image has some differences such as butterflies, but the image is not accurate of same scene it is left-shift of leftside image.</p>
</div>

<b>Prompt 3</b>
<pre>"Generate a simple image pair which is of same scene not shifting of the scene towards 
left/right and having some differences in the pair of images."
</pre>
<div >
  <p><p>Resulting image</p></p>
  <div align='center'>
    <img width="600" height='500' alt="image" src="https://github.com/user-attachments/assets/d9636257-ad80-4475-947e-e028ba43eddd">
  </div>
  <p><b>Image-7:</b> The best image in the set of images generated by the copilot for spotting difference puzzles which is having a slight shift in image pair and having some differences between them, but the image is not a sharp-edged image or unclear image.
</p>
</div>

<h3>3. Image generation tool based on prompts, by Google Gemini.</h3>
<b>Prompt 1</b>
<pre>"Can you generate two image that can be used for "spotting differences between the images " puzzles
the image should only generate the objects in it are person, bird, cat, cow, dog, horse, sheep, aeroplane,
bicycle, boat, bus, car, motorbike, train,bottle, chair, dining table, potted plant, sofa, tv/monitor."
</pre>
<div >
  <p><p>Resulting image</p></p>
  <div align='center'>
    <img width="600" height='600' alt="image" src="https://github.com/user-attachments/assets/d90ec300-fd6d-4abe-b67c-f962782c044f">
  </div>
  <p><b>Image-8:</b> It just generated an image containing of six things mentioned above.</p>
</div>

<b>Prompt 2</b>
<pre>"Generate a simple image pair which is of same scene not shifting of the scene towards left/right and
having some differences in the pair of images."
</pre>
<div >
  <p><p>Resulting image</p></p>
  <div align='center'>
    <img width="600" height='500' alt="image" src="https://github.com/user-attachments/assets/fb64d08a-cbe1-4cda-b3f7-e729a38175c3">
  </div>
  <p><b>Image-9:</b> It looks like the image is just scrapped from web and made a little shift in image pair with having no differences between them. It just changed the filter of the first image looks like rightside image is brighter than left.</p>
</div>

<h3>4. text2img: AI image generator based on prompts, by DeepAI</h3>
<b>Prompt 1</b>
<pre>"Generate two image that can be used for "spotting differences between the images " 
the image should only generate the objects in it are person, bird, cat, cow, dog, horse,
sheep, aeroplane, bicycle, boat, bus, car, motorbike, train, bottle, chair, dining table, 
potted plant, sofa, tv/monitor."
</pre>
<div >
  <p><p>Resulting image</p></p>
  <div align='center'>
    <img width="600" height='300' alt="image" src="https://github.com/user-attachments/assets/41c85adf-f4ac-4ce4-87e3-0af7fa1809df">

  </div>
  <p><b>Image-10:</b> The above image is generated by the DeepAI the above two are two different scenes. </p>
</div>

<b>Prompt 2</b>
<pre>"The same prompt as before is used but the tool have an option to generated image giving priority for quality of image 
instead of speed where the first is generated on speed."
</pre>
<div >
  <p><p>Resulting image</p></p>
  <div align='center'>
    <img width="600" height='300' alt="image" src="https://github.com/user-attachments/assets/1406cc69-a358-4c58-a7f4-1be031fe8861"><br/>
    <text align='center'> Image-11</text><br/><br/>
    <img width="600" height='300' alt="image" src="https://github.com/user-attachments/assets/6c2aafab-f17c-4fcd-8b8a-1ac30e6b11c9"><br/>
    <text align='center'>Image-12</text>
  </div>
  <p>Images 11,12 are generated by DeepAI image generator where intentionally a thing is placed in the scene by the generator which does not look like solving difference puzzles.
 </p>
</div>


<h3>5. AI Image Generator: The image generation based on prompts, by Canva</h3>
<b>Prompt 1</b>
<pre>"Generate two image that can be used for "spotting differences between the images " 
the image should only generate the objects in it are person, bird, cat, cow, dog, horse,
sheep, aeroplane, bicycle, boat, bus, car, motorbike, train, bottle, chair, dining table, 
potted plant, sofa, tv/monitor."
</pre>
<div >
  <p><p>Resulting image</p></p>
  <div align='center'>
    <img width="600" height='600' alt="image" src="https://github.com/user-attachments/assets/a2c6d67e-9a90-452c-be0c-d81249f1b8d1"><br/>
    <text align='center'><b>Image-13</b></text><br/><br/>
    <img width="600" height='600' alt="image" src="https://github.com/user-attachments/assets/0be7467b-fc7d-413f-a3bf-686117434eb1"><br/>
    <text align='center'><b>Image-14</b></text>
  </div>
  <p>Images 13,14 are generated by Canva where it roughly tried to generate the puzzle images, also the image are not clear and having semi generation of a completed object.
 </p>
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
<p>Now our task is to get the sub regions of both the image at given contours. Contours are extracted from the difference image that we generated in above step. For each of the sub region from both the images, we would like to detect if there are any objects in the sub region. If yes, we store the results for further clue generation.</p>
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
    <p>We see that the clues are generated by the program. If we closely look at the clues, we got three of our major clues that were already detected by the model: fire hydrant, bicycle, traffic light. The hot-air balloon got unrecognized and we believe this had happend because the model itself was not trained on detecting the hot-air balloons. Additionally we also got a clue that 'bird from top left' is different in images which is indeed different as in image1 the bird is half visible and in image2 the same bird in visible clearly</p>
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
    <p>Analysing the clues, we could infer that the expected objects are detected by the model at mentioned places.</p>
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

<h3>Testing on a real images</h3>
<p>We got the below image from the internet. We used yolov8m model to detect differences in this image </p>
<div align='center'>
  <img width=600' src='https://github.com/user-attachments/assets/ae85a9a8-b24b-494d-8f85-1161aa0f6606'/>
</div>
<p>The difference image generated is below</p>
<div align='center'>
  <img width='600' src='https://github.com/user-attachments/assets/eaf3e0a1-650d-48f8-beea-5aa3db49ac9e'/>
</div>

<b>The output</b>
<div align='center'>
  <img width="600" alt="yolo8m-people" src="https://github.com/user-attachments/assets/41f1c8dd-22cd-46a1-9989-40c9a878387c">
</div>
<p><b>Analysis</b></p>
<p>The above image has many clues in which most of them are correct detections. For example, the most easily detectable difference is the traffic light, dog, suitcases and persons, traffic cone. It even detected the cars in back. This increase in detection of objects is because the images are exactly copy of each other in terms of image dimensions and static indifferent objects. Hence the difference mask is generated accurately, helping to capture the most of the contours at correct places. Also the objects in the image are outlined sharply.</p>


<h2>Conclusion</h2>
<ul>
  <li>We have tried to generate similar images from the latest computer vision models that are available to us on the internet. We tried out variety of providers like DALL-E, Copilot, Gemini, DeepAI, text2Img.</li>
  <li>The images generated by DALL-E were better fit to implement our idea</li>
  <li>We tried out YOLOv8m and YOLO11n models to generate the clues at the end by detecting the different objects in the images</li>
  <li>We can evidently say that the best results are generated by the YOLOv8m model</li>
  <li>We also tested the differences in real image, which often gave better results when compared to AI generated images</li>
  <li>In conclusion, we infer that this game can be better implemented with greater accuracy in generating the clues from 'real' images</li>
</ul>

<h2>Future Enhancements</h2>
<p>
  To improve the gaming experience, we would like to create this web application as a full fledge game. Enhancements can be made by updating the SOTA models in both generating images as well as in object detections.
</p>

<h2>DALL-E generated 'similar' images</h2>
<div align='center'>
<img width='600' src='https://github.com/user-attachments/assets/6ff70b60-57e9-41ac-9a7b-7ba4c5a8b1f2'/>
<br/>
<img width='600' src='https://github.com/user-attachments/assets/3579dbc7-14f5-4d8e-b51c-9f58a24ccd03'/>
<br/>

  <img width='600' src='https://github.com/user-attachments/assets/34acd6ec-c3ad-4afc-865f-7c634512b289'/>
</div>
<div>
<p>But we are unable to use this images because these images are not clear, less sharpness of images,little brighter or any other fliter is applied with respective to its clone image, some images that are generated have difference in angle of light which effects visibility of one object by other object's shadows that can lead to mispredictions and also some are generated with no differences</p>


</div>
<div>
  
<b>Reference :</b>

<p><b>1.</b>https://chatgpt.com/ for DALL-E</p>
<p><b>2.</b>https://gemini.google.com/app for Imagen 3</p>
<p><b>3.</b>https://copilot.microsoft.com/ for Copilot</p>
<p><b>4.</b>https://deepai.org/machine-learning-model/text2img for DeepAI</p>
<p><b>5.</b>https://www.canva.com/ai-image-generator/ for AI Image Generator by Canvo</p>
<p><b>6.</b>https://github.com/ultralytics/ultralytics?tab=readme-ov-file for YOLO MODELs</p>
<p><b>7.</b>https://udell.weebly.com/uploads/1/6/8/6/16860542/spot-the-difference-before-and-after_orig.jpg Real World Image taken from web</p>

</div>




