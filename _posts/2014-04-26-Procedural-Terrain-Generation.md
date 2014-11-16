---
layout: post
permalink: '/:title'
title: Procedural Terrain Generation
category: Unity3, Graphics
tags:
- 3D
- Computer Graphics
- Game
- Game Egine
- Graphics
- Ken Perlin
- Perlin Noise
- Simple Noise
- Voxels
- Unity3D
author: Alex Catalán
---

<p style="text-align:justify;">
	The first thing we should ask ourselves is: What is the procedural content generation and what we can do with it on games? PCG is the the automatic or computer-assisted generation of game content such as levels, landscapes, items, rules, quests etc. This time we are generating the terrain for a posible sandbox game using voxels, like Minecraft. I have decided to use voxels because they are easy to manage and will allow me to do whatever I want with the terrain (cliffs, mountains, canyons, caves, corridors between caves, etc)
</p>
<p style="text-align:justify;">
<!--more-->
</p>
<div>
<h2>
	Noise
</h2>

<p style="text-align:justify;">
{% img alignright /assets/2014-04-26-Procedural-Terrain-Generation/SignalNoise.png 212 %}
	<!--<a href="http://www.madowen.es/wp-content/uploads/2014/04/SignalNoise.png" rel="" style="font-size:13px;text-align:justify;" target="" title=""><img alt="Signal Noise" class="alignright size-full wp-image-103" height="158" src="http://www.madowen.es/wp-content/uploads/2014/04/SignalNoise.png" title="Signal Noise" width="212" /></a>-->To generate our terrain we will use a noise signal. What is noise? Noise is a general term for unwanted modifications that a signal may suffer. This definittion is usually for processing audio signals but in computer graphics noise is used to generate realistic contents. There are different algorithms to make noise, but the most common is Perlin Noise developed by Ken Perlin, who won an Academy Award for Technical Achievement for inventing it.
</p>

<h3 style="text-align:justify;">
	Perlin Noise
</h3>

<p style="text-align:justify;">
	Perlin Noise is most commonly implemented as a 2D, 3D or 4D function, but can be defined for any number of dimensions.
</p>

<p style="text-align:justify;">
	The algorithm takes a noise function to generate a number of samples with different frequency and amplitude, these samples are summed to generate a new signal, you can see it on the next images in 1D and 2D.
</p>

<p style="text-align:justify;">
{% img /assets/2014-04-26-Procedural-Terrain-Generation/Perlin-Noise-1D.png %}
	<!--<a href="http://www.madowen.es/wp-content/uploads/2014/04/Perlin-Noise-1D.png" rel="" style="font-size:13px;text-align:justify;" target="" title=""><img alt="Perlin Noise 1D" class="size-full wp-image-103 aligncenter" height="99" src="http://www.madowen.es/wp-content/uploads/2014/04/Perlin-Noise-1D.png" title="Perlin Noise 1D" width="963" /></a>-->
</p>

<p style="text-align:justify;">
	As you can see, every sample adds a perturbation to the first one, being the first with greater amplitude the most significant, the next ones have less amplitude but greater frequency, which is what adds the disturbing values to the signal.
</p>

<p style="text-align:justify;">
{% img /assets/2014-04-26-Procedural-Terrain-Generation/Perlin-Noise-2D.png %}
	<!--<a href="http://www.madowen.es/wp-content/uploads/2014/04/Perlin-Noise-2D.png" rel="" style="font-size:13px;text-align:justify;" target="" title=""><img alt="Perlin Noise 2D" class="size-full wp-image-103 aligncenter" height="99" src="http://www.madowen.es/wp-content/uploads/2014/04/Perlin-Noise-2D.png" title="Perlin Noise 2D" width="963" /></a>-->
</p>

<p style="text-align:justify;">
	Knowing this, the Perlin Noise function comes from basically 2 parameters apart from the N coordenates in N-Dimension noise, the Persistance and the Octaves. Each successive noise function you add is know as an Octave and the Persistance deffines the amplitude of each Octave.
</p>

<p style="text-align:justify;">
	\(frequency = 2^i\)<br />
	\(amplitude = persistence^i\)
</p>

<h3 style="text-align:justify;">
	Simplex Noise
</h3>

<p style="text-align:justify;">
	Now that we know the basics of Perlin Noise, Ken Perlin in 2001 designed an algorithm to address the limitations of his classic noise function, especially in higher dimensions. The Simplex Noise is a method for constructing an n-dimensional noise function comparable to Perlin Noise but with a lower computational overhead, especially in larger dimensions.<br />
	The advantages of simplex noise over Perlin noise:
</p>

<ul>
	<li>
		Simplex noise has a lower computational complexity and requires fewer multiplications.
	</li>
	<li>
		Simplex noise scales to higher dimensions (4D, 5D) with much less computational cost, the complexity is \(O(n^2)\) for n dimensions instead of the \(O(2^n)\) of classic noise.
	</li>
	<li>
		Simplex noise has no noticeable directional artifacts (is isotropic).
	</li>
	<li>
		Simplex noise has a well-defined and continuous gradient everywhere that can be computed quite cheaply.
	</li>
	<li>
		Simplex noise is easy to implement in hardware.
	</li>
</ul>

<p>
	 
</p>

<p style="text-align:justify;">
	These are the reasons why I chose Simplex Noise instead of Perlin Noise. The values ​​obtained are virtually the same but the algorithm is better.
</p>
</div>
<div>
<h2>
	Terrain
</h2>

<p style="text-align:justify;">
{% img alignright /assets/2014-04-26-Procedural-Terrain-Generation/Noise.jpg 212 %}
	<!--<a href="http://www.madowen.es/wp-content/uploads/2014/04/Noise.jpg" rel="" style="font-size:13px;text-align:justify;" target="" title=""><img alt="Noise" class="alignright size-full wp-image-103" height="158" src="http://www.madowen.es/wp-content/uploads/2014/04/Noise.jpg" title="Noise" width="212" /></a>-->Once we have defined the noise algorithm with the Persistance and the Octaves, we simply create a voxel at coordinates (X, Y, Z), where the Y-coordinate is the value obtained by the noise function. As the value comes between (-1, 1) we sum 1 and divide by 2 to get a value between (0, 1). Now we simply multiply that value by the maximum height that we want to our terrain.
</p>

<p style="text-align:justify;">
	This is what you get with a Persistence of 0.27 and 3 Octaves, I have divided the terrain in sectors because of limitations of Unity, there are 10x10 sectors with 20x20 cubes per sector (around 40k cubes), but this is not really important to see what we want to.
</p>
<a href="/assets/2014-04-26-Procedural-Terrain-Generation/MaDCraft-Linear.html">{% img /assets/2014-04-26-Procedural-Terrain-Generation/MaDCraft-Linear-Player.jpg %}</a>

<h3>
	Biomes
</h3>

<p style="text-align:justify;">
{% img alignright /assets/2014-04-26-Procedural-Terrain-Generation/Biome-Linear.png 212 %}
	<!--<a href="http://www.madowen.es/wp-content/uploads/2014/04/Biome-Linear.png" rel="" style="font-size:13px;text-align:justify;" target="" title=""><img alt="Biome - Linear" class="alignright size-full wp-image-103" height="142" src="http://www.madowen.es/wp-content/uploads/2014/04/Biome-Linear.png" title="Biome - Linear" width="191" /></a>-->Here we have it, our procedurally generated terrain using voxels and Simplex noise but... is pretty boring, it would be great if it had some cliffs, canyons, high mountains, etc. To do that we have to create some biomes.
</p>

<p style="text-align:justify;">
	Biomes, taking Minecraft as model, are regions with varying geographical features, flora, heights, temperatures, humidity ratings, and sky and foliage colors. From now on we will focus our work on heights, using a curve values we can modify the height of our terrain and create completly different terrains with the same noise parameters.{% img alignright /assets/2014-04-26-Procedural-Terrain-Generation/Biome-Cliff.png 212 %}<!--<a href="http://www.madowen.es/wp-content/uploads/2014/04/Biome-Cliff.png" rel="" style="font-size:13px;text-align:justify;" target="" title=""><img alt="Biome - Cliff" class="alignright size-full wp-image-103" height="142" src="http://www.madowen.es/wp-content/uploads/2014/04/Biome-Cliff.png" title="Biome - Cliff" width="191" /></a>-->
</p>

<p style="text-align:justify;">
	The first curve on the right, is a straight line from 0 to 1, this means that if you get a value from the noise function equals to 0.1 and you evaluate this value with the curve, you will get the same value 0.1, in other words this is the curve used at the example seen before. 
</p>

<p style="text-align:justify;">
	The other curve is quite different, more irregular, made by me. This curve will change the values obtained from the noise function significantly. When we have values from 0 to 0.52 the curve will return values near to 1, the values from 0.52 to 0.9 will be around 0.45, and from 0.9 to 1 will fall to 0.3. If we run the example with this curve what we see is a terrain made of cliffs and plains.
</p>
<a href="/assets/2014-04-26-Procedural-Terrain-Generation/MaDCraft-Cliff.html">{% img /assets/2014-04-26-Procedural-Terrain-Generation/MaDCraft-Cliff-Player.jpg %}</a>
<p style="text-align:justify;">
	That's all for now, the next goal should be add some textures to the cubes, figure out how to create a huge map where more than one biome enter to the game and how to deal with the transitions between biomes.
</p>
</div>