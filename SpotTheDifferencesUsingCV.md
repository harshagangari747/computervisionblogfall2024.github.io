<h2 align="center"> <b></b>Solving 'SPOTTING THE DIFFERENCES' puzzle using Computer Vision </h2></p>
<p align="center"><i><b>Sai Harsha Gangari, &nbsp; Sairam Reddy Malreddy</b></i></p>

<h4>Introduction</h4>
<p>Spotting the differences in two similar images is a fun game to solve. But sometimes, this game can become more difficult and tricky to solve as an adult. We got an idea to solve this game using computer vision techniques and advancements that are made in the recent years. This game is all about spotting the objects which are modified in some or the other way. For example: change in color, missing object or misplaced object from one image etc. This lead us to chose a object detection model called <b>YOLO</b> which was released in the year 2020.</p>
<p> We designed a website that gives a user a pair of 'similar' images. The user needs to detect the objects that are different in both the images. If the user can't figure out any differences, we give a set of 'clues' to guide the user to spot the differences. The image and clue generation is made using computer vision models; YOLO and DALL-E</p>

<h4>Objective</h4>
<p>Given a pair of two similar images, the objective is to generate clues to guide player to spot the differences in both the images. The clues could be about the objects itself or position of the objects</p>

<h4>Approach</h4>
<p>Our approach includes three steps.
<ul>
  <li>Get the pair of two 'similar' images from DALL-E by prompting the description of the image scene</li>
  <li>Using <i><b>'opencv'</b></i> python package, find the pixel differences in both the images</li>
  <li>Get the contour regions as the 'region of interest' in the difference-image</li>
  <li>Find the object in both the images at the given contour regions using YOLO model</li>
  <li>From the obtained objects that were detected, generate clues</li>
  </ul>
</p>

<h4>Architecture</h4>
<p> The following image briefly depicts the architecture of the program</p>
<img align="center" src="https://github.com/user-attachments/assets/c7ba3502-03e8-464b-9bd5-db4cdcc948e7" alt="Architecture Diagram" />
