---
layout: post
permalink: '/:title'
title: Octree + Ray Collision
category: Computational Geometry
tags:
- 3D
- Computational Geometry
- Octree
- OpenGL
- Quadtree
- Ray Collision
- Teapot	
author: Alex Catalán
---

<p style="text-align:justify;">
	Nowadays in applications that work with 3D graphical environments require monitoring the collision between meshes with lots of triangles, so that the computational cost of checking all triangles of both meshes would be too high.
</p>
<p style="text-align:justify;">
	To solve this problem, it&#039;s posible to divide the triangles of the mesh in areas, so we will have a smaller amount of triangles to check for each sector. This creates a data structure in a tree, in which the father wraps to subsectors children. This distribution of data also can be applied in both 2D (Quadtree) and 3D (Octree).
</p>
<p id="more">
	<!--more-->
</p>
<div>
	<h2>
		2D Quadtree
	</h2>
	<p style="text-align:justify;">
		Before starting to explain how a Octree works, you must understand how a Quadtree works and how is generated its tree.
	</p>
	<p style="text-align:justify;">
		In fact what a quadtree and octree they do is distribute the vertices between their children, then the triangles are distributed according to where they are situated its vertices.
	</p>
	<p style="text-align:justify;">
		First of all the vertices are distributed on the parent node (Node 0), as seen in the image, the nodes are within the black square.
	</p>
	<p style="text-align:justify;">
		{% img /assets/2013-05-24-Octree-+-Ray-Collision/Quadtree.gif %}
	</p>
	<p style="text-align:justify;">
		Once the vertices have spread over the parent node is divided into subsectors alike. Since this is a 2D case, will be divided between two coordinates X and Y creating 4 new subsectors.
	</p>
	<p style="text-align:justify;">
		For each new node that has been created, will distribute the vertices of the parent node in the child nodes, so that the parent node runs out vertices.
	</p>
	<p style="text-align:justify;">
		Understand this, we repeat the same process recursively. The process is repeated for the child nodes until it finds a node that has zero or one vertex, or up to the specified maximum depth.
	</p>
</div>
<div>
	<h2>
		3D Octree
	</h2>
	<p style="text-align:justify;">
		Now that we know how a Quadtree for 2D environments works, implement an Octree for 3D environments follows the same process.
	</p>
	<p style="text-align:justify;">
		First, the vertices and the triangles from the mesh are added to the Octree al level 0 and calculates its Bounding Box (a Bounding Box is the minimum sized box that can hold all the vertices).
	</p>
	<p style="text-align:justify;">
		{% img /assets/2013-05-24-Octree-+-Ray-Collision/Octree.gif %}
	</p>
	<p style="text-align:justify;">
		Now that Node 0 has the vertices, it must generate the child nodes. To divide the area it Node 0 (the bounding box), as in the quadtree which was divided by half in two dimensions, for Octree as it is 3D will divide into three dimensions. This will generate 8 new child nodes, which are Octrees, and will distribute the vertices and the triangles of the parent node to them.
	</p>
	<p style="text-align:justify;">
	{% img  /assets/2013-05-24-Octree-+-Ray-Collision/Octree-all-nodes.jpg %}
	<!--<a href="http://madowen.es/wp-content/uploads/2013/04/Octree-all-nodes.jpg" rel="" target="" title=""><img alt="Octree - all nodes" class="size-full wp-image-52 aligncenter" height="350" src="http://madowen.es/wp-content/uploads/2013/04/Octree-all-nodes.jpg" title="Octree - all nodes" width="963" /></a>-->
	</p>
	<p style="text-align:justify;">
		After that, simply repeat the process for the child nodes, and the child nodes of the child nodes recursively, until you reach the maximum depth or the node has no vertices.
	</p>
</div>
<div>
	<h2>
		Ray Collision
	</h2>
	<p style="text-align:justify;">
		One of the most widely used tests in the development of 3D environments, is the Ray Collision. This check has different uses.
	</p>
	<ul>
		<li style="text-align:justify;">
			<strong>Ray Picking</strong>: Clicking on the screen with the mouse, draws a ray from the camera to the point at the world for the screen, if the ray collides with an object, it will be selected.
		</li>
		<li style="text-align:justify;">
			<strong>Ray Tracing</strong>: Is a technique for generating an image by tracing the path of light through pixels in an image plane and simulating the effects of its encounters with virtual objects. Tracing a ray from the camera for each screen pixel is calculated if it collides with an object. In case of a collision with an object, you paint the pixel on the screen with the information of the surface that has collided.
		</li>
	</ul>
	<p style="text-align:justify;">
		{% img  /assets/2013-05-24-Octree-+-Ray-Collision/Octree-Ray-Collision-1.jpg %}
		<!--<a href="http://madowen.es/wp-content/uploads/2013/04/Octree-Ray-Collision-1.jpg" rel="" style="line-height:1.6em;" target="" title=""><img alt="Octree - Ray Collision 1" class="alignnone size-full wp-image-59" height="350" src="http://madowen.es/wp-content/uploads/2013/04/Octree-Ray-Collision-1.jpg" title="Octree - Ray Collision" width="963" /></a>-->
	</p>
	<p style="text-align:justify;">
		To optimize checking if a ray collides with a mesh in the world, you can use Octrees. If an octree is not used, it should check whether the ray collides with all the triangles of the mesh. On the other hand using Octree, it should check whether the ray collides with at most 8xN Bounding Boxes, where N is the maximum depth of the Octree. And once reached the maximum depth, check if the ray intersects any of the triangles contained in the node.
	</p>
	<p style="text-align:justify;">
	{% youtube hxLTe-twNb0 %}
	</p>
	<p style="text-align:justify;">
		Here is the source code: <a href="/assets/2013-05-24-Octree-+-Ray-Collision/Octree-src.zip"><i class="icon-download"></i>SRC</a>
	</p>
</div>