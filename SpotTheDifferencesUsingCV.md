<h2 align="center"> <b></b>Solving 'SPOTTING THE DIFFERENCES' puzzle using Computer Vision </h2></p>
<p align="center"><i><b>Sai Harsha Gangari, &nbsp; Sairam Reddy Malreddy</b></i></p>

<h3>Introduction</h3>
<p>Spotting the differences in two similar images is a fun game to solve. But sometimes, this game can become more difficult and tricky to solve as an adult. We got an idea to solve this game using computer vision techniques and advancements that are made in the recent years. This game is all about spotting the objects which are modified in some or the other way. For example: change in color, missing object or misplaced object from one image etc. This lead us to chose a object detection model called <b>YOLO</b> which was released in the year 2020.</p>
<p> We designed a website that gives a user a pair of 'similar' images. The user needs to detect the objects that are different in both the images. If the user can't figure out any differences, we give a set of 'clues' to guide the user to spot the differences. The image and clue generation is made using computer vision models; YOLO and DALL-E</p>

<h3>Objective</h3>
<p>Given a pair of two similar images, the objective is to generate clues to guide player to spot the differences in both the images. The clues could be about the objects itself or position of the objects</p>

<h3>Approach</h3>
<p>Our approach includes few steps.</p>
<ol>
  <li>
<h5>Collect images from different models / LLM's available on the internet</h5>
  <ul>
    <li>The first task is to generate a pair of images from the computer vision models available on the internet</li>
    <li>We considered DALL-E, Google Gemini, Microsoft's CoPilot</li>
  </ul>
    </li>
<li>
<h5>Input the images to the model for spotting the differences</h5>
<ul>
  <li>The second task is to feed the input to the next stage of the pipeline. Shortly, the pipeline is like </li>
  <ul>
      <li>First, using <i><b>'opencv'</b></i> python package, find the pixel differences in both the images</li>
      <li>Second, get the contour regions in the difference image. Get the part of the image1 and image2 as 'region of interest' that corresponds to the contours</li>
  </ul>
  </ul>
  </li>
  <li>
<h5> Find the clues using object detection</h5>
  <ul>
    <li>Third, find the object in both the subpart of the images</li>
      <li>Fourth, from the obtained objects that were detected in both the imagess, generate clues</li>
  </ul>
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


<p>Now we shall dive into the above three steps in detail</p>

<!-- ===================================================================Task 1 Begin =================================================================== -->
<h2>Task1: Collecting the Images</h2><h4>Issues</h4>

<p>In our first task, we queried many of computer vision models that are available out there on internet. We queried DALL-E, Google Gemini, Microsoft's Copilot. We realized that since we are using the YOLO model, we thought of keeping the objects that are only detectable by the this model. So for the prompts that we gave, we tried to include a list of objets that we are particularly interest, which were also detectable by YOLO </p>


<p>We need to give a reference image to DALL-E to make it understand that we are seeking two similar pictures with minor differences in it. Else DALL-E struggles to understand the context and gives images which are essentially different. We prompted DALL-E with the below detailed description of the objects to include init along with a reference image</p>


<p>Here is a detailed textual description of the image prompted to DALL-E</p>
<pre>Create an image of a scenery/landscape by taking the reference of above image. 
Use only objects from this list 0: 'person', 1: 'bicycle', 2: 'car', 3: 'motorcycle', 4: 'airplane', 5: 'bus', 6: 'train', 7: 'truck', 8: 'boat', 9: 'traffic light', 10: 'fire hydrant', 11: 'stop sign', 12: 'parking meter', 13: 'bench', 14: 'bird', 15: 'cat', 16: 'dog', 17: 'horse', 18: 'sheep', 19: 'cow', 20: 'elephant', 21: 'bear', 22: 'zebra', 23: 'giraffe', 24: 'backpack', 25: 'umbrella', 26: 'handbag', 27: 'tie', 28: 'suitcase', 29: 'frisbee', 30: 'skis', 31: 'snowboard', 32: 'sports ball', 33: 'kite', 34: 'baseball bat', 35: 'baseball glove', 36: 'skateboard', 37: 'surfboard', 38: 'tennis racket', 39: 'bottle', 40: 'wine glass', 41: 'cup', 42: 'fork', 43: 'knife', 44: 'spoon', 45: 'bowl', 46: 'banana', 47: 'apple', 48: 'sandwich', 49: 'orange', 50: 'broccoli', 51: 'carrot', 52: 'hot dog', 53: 'pizza', 54: 'donut', 55: 'cake', 56: 'chair', 57: 'couch', 58: 'potted plant', 59: 'bed', 60: 'dining table', 61: 'toilet', 62: 'tv', 63: 'laptop', 64: 'mouse', 65: 'remote', 66: 'keyboard', 67: 'cell phone', 68: 'microwave', 69: 'oven', 70: 'toaster', 71: 'sink', 72: 'refrigerator', 73: 'book', 74: 'clock', 75: 'vase', 76: 'scissors', 77: 'teddy bear', 78: 'hair drier', 79: 'toothbrush'.

Two image should be similar and copy of each other. But make changes to few objects for example, change the position of an object, change color of an object, delete an object, add a new object from the list. When I overlap the images, there should not be any pixel shift of the images. There should exactly align most of the static objects when overlapped. </pre>

<p>Here is the reference image that we gave along with the prompt to DALL-E </p>
<div align='center'><img width='50%' src = 'https://github.com/user-attachments/assets/7c97e6ff-4879-458f-b1a4-6cc494593156'/></div>

<p>And here is the image generated by the DALL-E</p>
<div align='center'><img width= '50%' src = 'https://github.com/user-attachments/assets/c75c91d9-cc21-42ee-89e7-9df948413b7a'/></div>

<p>The difference of images is key here to determine the contours so that we can extract the part of images where the objects that differ in both image can potentially lie.</p> <p>Here the differece image of the above DALL-E generated image is</p>

<h4>Issues:</h4>
<p>Here, the image generated by DALL-E is exceptionally similar when seen, but when we tried to overlap the images on each other, there is significant pixel shift of the same object in both the images. For example, the cactus in both the images are at same height and same size, however, upon super positioned one image on another, the cactuses are at a different position than other.</p>

<div align='center' height='50%'><img height=430px width=350px src='https://github.com/user-attachments/assets/07879f06-d635-415a-a66f-1fa87ed3e571'/></div>

<p>When we used this image and fed into the model we got the output clues as</p>

<div align='center' height='60%' width='50%'><img width='1000px' src='https://github.com/user-attachments/assets/0ebc8f44-5448-4c07-b6f0-d8287b2b60af'/></div>
<h4>Analysis</h4>
As we can see that, model has generated clues. But if we take a look at few clues, we understand that those objects infered in the clues have never appeared in the input image. We believe this has happend because of in accurate generation of 'difference image'. This inaccuracy is caused due to scene shift in both images generated by DALL-E. We tried to crop the image to compensate the scene shift, but to align the outline of static/indifferent objects to get 0 in difference image, we had to stretch or shrink one of the images. If we do so, few object's outline misalign if not other objects will. This lead to the difference images such as the above one.

If we manually verify the differences, it is evident that fire hydrant, cycle in the bottom right are differences in both the image which the model found. There are hot balloon,traffic lights and light pole different in images, but model didn't figure out these differences. The model thought that the cactus 'long head' was a surfboard, as it resembles it in the shape of surfboard. 
<h2> Task 2: Spot the differences by inputing the images to the model</h2>
<p>Let us look at another example</p>
<h4>Prompt</h4>
<pre>ds</pre>

<p> The image generated by DALL-E is </p>
<div align='center' height='50%'><img width='50%' src='https://github.com/user-attachments/assets/40d4a648-def7-4f20-b78c-3f6d0a6bc91e'/></div>

<p>The clues generated by the program are</p>
<div align='center'><img width="1511"  src="https://github.com/user-attachments/assets/279748fb-3e8a-4e85-9243-e243c1e6e1e6"></div>

<p>The difference generated is</p>
<div align='center'><img height=350px' width='300px' src="https://github.com/user-attachments/assets/cbcc23c9-95e1-473b-a34e-12c7bddc5c4c"/></div>

<h4>Analysis</h4>
We can see there are many clues generated by the model. The bottle in the center (green), dog behind, flowerpot behind the dog, brown flower pot in the bottom right in image2 are different evidently different in the image which the model detected. The model detected the dog as teddy bear.

<!-- ===================================================================Task 2 Begin =================================================================== -->
<h2>Task 2: Finding the difference Image</h2>
<p>Difference image is key for us as we draw the boundaries/contours of the parts of both the images to get the sub region in the image which we feed to the YOLO model for object detection. We used python's <b>opencv-python</b> package to find the difference mask as well as the contours in the images. We used the `absdiff` method and  `findContours` methods to obtain the difference images as well as the contours from both the images respectively.  </p>
<p>Here are some difference images that the program produced. These are the images that are produced using DALL-E</p>
<table>
  <tr height="50%">
    <td><img height=350px width=300px src='https://github.com/user-attachments/assets/07879f06-d635-415a-a66f-1fa87ed3e571'/></td>
    <td><img height=350px width=300px src="https://github.com/user-attachments/assets/cbcc23c9-95e1-473b-a34e-12c7bddc5c4c"/></td>
    <td><img height=350px width=300px src="https://github.com/user-attachments/assets/181e6c91-527c-4130-8cca-a6dcae84d099"/></td>

  </tr>
</table>








