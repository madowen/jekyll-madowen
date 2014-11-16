---
layout: post
permalink: '/:title'
title: QuickHull 3D
category: Computational Geometry
tags:
- 3D
- Computational Geometry
- OpenGL
- Quickhull
- Teapot
author: Alex Catalán
---

<p style="text-align:justify;">
	At the Computational Geometry course I have implemented the Quickhull algorithm for its application in 3D, following the paper <em>&quot;Barber, C. B., Dobkin, D. P., &amp; Huhdanpaa, H. T. (1996). The Quickhull algorithm for convex hulls. ACM Trans. on Mathematical Software, 22(4), 469&mdash;483&quot;.</em> And following the example of <a href="http://www.cse.unsw.edu.au/~lambert/java/3d/hull.html?dimension=3D&amp;model=QuickHull&amp;seed=45123&amp;npoints=20&amp;frame=11&amp;view=v[0.55695075,-0.051542707,-0.8289446]" target="_blank" title="QuickHull">this java applet</a> developed by UNSW School of Computer Science and Engineering.
</p>
<p id="more">
	<!--more--> 
</p>
<div>
	<h2>
		<strong>Algorithm</strong>
	</h2>
	<p style="text-align:justify;">
		The algorithm proposed by the paper says:
	</p>
	<blockquote>
		<p style="text-align:justify;">
			<span style="font-family: georgia, serif; font-style: italic;">create a simplex of </span><em style="font-family: georgia, serif;">d</em><span style="font-family: georgia, serif; font-style: italic;">+1 points</span>
		</p>
		<p style="text-align:justify;">
			<span style="font-family: georgia, serif; font-style: italic;">for each facet </span><em style="font-family: georgia, serif;">F</em>
		</p>
		<p style="margin-left: 40px;">
			<span style="font-family: georgia, serif; font-style: italic;">for each unassigned point </span><em style="font-family: georgia, serif;">p</em>
		</p>
		<p style="margin-left: 80px;">
			<span style="font-family: georgia, serif; font-style: italic;">if </span><i style="font-family: georgia, serif;">p</i><span style="font-family: georgia, serif; font-style: italic;"> is above </span><em style="font-family: georgia, serif;">F</em>
		</p>
		<p style="margin-left: 120px;">
			<span style="font-family: georgia, serif; font-style: italic;">assign </span><em style="font-family: georgia, serif;">p</em><span style="font-family: georgia, serif; font-style: italic;"> to </span><em style="font-family: georgia, serif;">F</em><span style="font-family: georgia, serif; font-style: italic;">&#39;s outside set</span>
		</p>
		<p style="text-align:justify;">
			<span style="font-family: georgia, serif; font-style: italic;">for each facet </span><em style="font-family: georgia, serif;">F</em><span style="font-family: georgia, serif; font-style: italic;"> with a non-empty outside set</span>
		</p>
		<p style="margin-left: 40px;">
			<span style="font-family: georgia, serif; font-style: italic;">select the furthest point </span><em style="font-family: georgia, serif;">p</em><span style="font-family: georgia, serif; font-style: italic;"> of </span><em style="font-family: georgia, serif;">F</em><span style="font-family: georgia, serif; font-style: italic;">&#39;s outside set</span>
		</p>
		<p style="margin-left: 40px;">
			<span style="font-family: georgia, serif; font-style: italic;">initialize the visible set </span><em style="font-family: georgia, serif;">V </em><span style="font-family: georgia, serif; font-style: italic;">to </span><em style="font-family: georgia, serif;">F</em>
		</p>
		<p style="margin-left: 40px;">
			<span style="font-family: georgia, serif; font-style: italic;">for all unvisited neighbors </span><em style="font-family: georgia, serif;">N</em><span style="font-family: georgia, serif; font-style: italic;"> of facets in </span><em style="font-family: georgia, serif;">V</em>
		</p>
		<p style="margin-left: 80px;">
			<span style="font-family: georgia, serif; font-style: italic;">if </span><em style="font-family: georgia, serif;">p</em><span style="font-family: georgia, serif; font-style: italic;"> is above </span><em style="font-family: georgia, serif;">N</em>
		</p>
		<p style="margin-left: 120px;">
			<span style="font-family: georgia, serif; font-style: italic;">add </span><em style="font-family: georgia, serif;">N</em><em style="font-family: georgia, serif;"> </em><span style="font-family: georgia, serif; font-style: italic;">to </span><em style="font-family: georgia, serif;">V</em>
		</p>
		<p style="margin-left: 40px;">
			<span style="font-family: georgia, serif; font-style: italic;">the boundary of </span><em style="font-family: georgia, serif;">V</em><span style="font-family: georgia, serif; font-style: italic;"> is the set of horizon ridges </span><em style="font-family: georgia, serif;">H</em>
		</p>
		<p style="margin-left: 40px;">
			<span style="font-family: georgia, serif; font-style: italic;">for each ridge </span><em style="font-family: georgia, serif;">R</em><span style="font-family: georgia, serif; font-style: italic;"> in </span><em style="font-family: georgia, serif;">H</em>
		</p>
		<p style="margin-left: 80px;">
			<span style="font-family: georgia, serif; font-style: italic;">create a new facet from </span><em style="font-family: georgia, serif;">R</em><span style="font-family: georgia, serif; font-style: italic;"> and </span><em style="font-family: georgia, serif;">p</em>
		</p>
		<p style="margin-left: 80px;">
			<span style="font-family: georgia, serif; font-style: italic;">link the new facet to its neighbors</span>
		</p>
		<p style="margin-left: 40px;">
			<span style="font-family: georgia, serif; font-style: italic;">for each new facet </span><em style="font-family: georgia, serif;">F&#39;</em>
		</p>
		<p style="margin-left: 80px;">
			<span style="font-family: georgia, serif; font-style: italic;">for each unassigned point </span><em style="font-family: georgia, serif;">q</em><span style="font-family: georgia, serif; font-style: italic;"> in an outside set of a facet in </span><em style="font-family: georgia, serif;">V</em>
		</p>
		<p style="margin-left: 120px;">
			<span style="font-family: georgia, serif; font-style: italic;">if </span><em style="font-family: georgia, serif;">q</em><span style="font-family: georgia, serif; font-style: italic;"> is above </span><em style="font-family: georgia, serif;">F&#39;</em>
		</p>
		<p style="margin-left: 160px;">
			<span style="font-family: georgia, serif; font-style: italic;">assign </span><em style="font-family: georgia, serif;">q </em><span style="font-family: georgia, serif; font-style: italic;">to </span><em style="font-family: georgia, serif;">F&#39;</em><span style="font-family: georgia, serif; font-style: italic;">&#39;s outside set</span>
		</p>
		<p style="margin-left: 40px;">
			<span style="font-family: georgia, serif; font-style: italic;">delete the facets in </span><em style="font-family: georgia, serif;">V</em>
		</p>
	</blockquote>
	<p style="text-align:justify;">
		I made some changes on my implementation but the essence is the same.
	</p>
	<p style="text-align:justify;">
		{% img /assets/2013-04-13-QuickHull-3D/quickhull-initial.jpg %}
	</p>
</div>
<div>
	<h2>
		<strong>Data Structures</strong>
	</h2>
	<p style="text-align:justify;">
		Before explaining the implementation, it should explain how are the data structures.
	</p>
	<ul>
		<li style="text-align: justify;">
			<code>vector3f </code>is a 3 <code>float </code>vector with some geometrical functions (dotProduct, crossProduct, etc...)
		</li>
		<li style="text-align: justify;">
			<code>facet </code>is a 3 <code>int </code>vector and a <code>std::Vector&lt;int&gt;</code>, to store the indices of the 3 points of the triangle and the indices of the outside points.
		</li>
		<li style="text-align: justify;">
			All the collection used are class <code>std::Vector</code>
		</li>
		<li style="text-align: justify;">
			The QuickHull class has:
			<ul>
				<li>
					A points collection is a <code style="line-height: 1.6em;">std::Vector &lt;vector3f&gt;</code>, where there are stored all the points. This should not be changed any time.
				</li>
				<li>
					A facets collection is a <code style="line-height: 1.6em;">std::Vector &lt;facet&gt;</code>, where there are stored all the facets.
				</li>
			</ul>
		</li>
	</ul>
	<h2>
		<strong>Implementation</strong>
	</h2>
	<p style="text-align:justify;">
		The first thing to do is calculate the simplex. The simplex is a triangle that separates points in two areas. This will take 3 points, two with maximum X and Y coordinate, and another with the minimum Z coordinate of the whole set of points.
	</p>
	<p style="text-align:justify;">
		{% img /assets/2013-04-13-QuickHull-3D/quickhull-simplex.jpg %}
	</p>
	<p style="text-align:justify;">
		After the simplex is created, split the remaining points that are visible for the two facets from the simplex (a triangle in 3D has 2 faces). To know if a point is visible for a facet, we have to calculate if this point is above the facet. And to know if a point is above a plane, we evaluate the point on the plane normal.
	</p>
	<p style="text-align:justify;">
		Now the main loop begins. In each iteration and for each facet, the facet&#39;s furthest point is calculated. This point <em style="font-size: 13px;">p</em> is checked whether is visible from the facets neighbourhood, the neighbourhood facets are the ones that have at least one vertex in common, then all ridges and outside points from the neighbourhood facets are saved in different collections.
	</p>
	<p style="text-align:justify;">
		The ridge collection determine the boundary from the hole that is created when the neighbourhood facets are deleted. Using this boundary and the furthest point <em style="font-size: 13px;">p</em> will create new triangles using the Delaunay algorithm. 
	</p>
	<p style="text-align:justify;">
		Finally once created the new triangles will split the points of collection points on the new triangles created. This loop is executed while there are facets with outside points.
	</p>
	<p style="text-align:justify;">
		{% img /assets/2013-04-13-QuickHull-3D/quickhull-final.jpg %}
	</p>
</div>
<div>
	<h2>
		<strong>Solved Facts</strong>
	</h2>
	<h3>
		<strong>Triangle orientation</strong>
	</h3>
	<p style="text-align:justify;">
		One of the problems that can happen when you are adding new triangles on a closed polygon, is that according to the order of the vertices of the triangles, the normal of the triangle is pointing inside the polygon. This is a problem because to perform calculations to find the location of the points on the triangle requires that the normal is facing outside the polygon.
	</p>
	<p style="text-align:justify;">
		The solution implemented, was using a reference point. This point must always remain within the polygon, even in the first iteration. Therefore, is calculated the baricenter of the initial simplex.
	</p>
	<p style="text-align:justify;">
	{% youtube 9Nx3GaHteCc %}
	</p>
	<p style="text-align:justify;">
		Here is the source code: <a href="http://www.madowen.es/wp-content/uploads/2014/01/QuickHull-src.zip"><i class="icon-download"></i>SRC</a>
	</p>
</div>