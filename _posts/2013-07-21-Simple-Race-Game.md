---
layout: post
title: Simple Race Game
category: Interactive Systems
tags:   2D, Car, Game, Interactive Systems, Race
author: Alex Catalán
---

<p style="text-align:justify;">
	In the last quarter of this year in college, I am studying Interactive Systems. In this course we started working with OpenFrameworks, which is nothing more than a framework that makes it easy to use OpenGL and development of 2D and 3D Applications. The first project that we are developing is a 2D multiplayer racing game.
</p>

<p>
	<!--more-->
</p>

<div>
    <h2>
       Framework
   </h2>

   <p style="text-align:justify;">
       First we have to download from <a href="http://www.openframeworks.cc/download/">OpenFrameworks</a> website the latest release for the IDE we use, in this case we use Code Blocks. Once downloaded and unzipped wherever you want, go to the folder /apps/myApps/emptyExample, there you will find what you need to start immediately. If we open the file emptyExample.workspace with Code Blocks, we can see 3 files created in the folder /src.
   </p>

   <pre>
    #include "testApp.h"
    #include "ofAppGlutWindow.h"

    //--------------------------------------------------------------
    int main(){
        ofAppGlutWindow window; // create a window
        // set width, height, mode (OF_WINDOW or OF_FULLSCREEN)
        ofSetupOpenGL(&amp;window, 1024, 768, OF_WINDOW);
        ofRunApp(new testApp()); // start the app
    }
    {% endhighlight %}

    <p style="text-align:justify;">
    	In the main.cpp file, basically is created a window with the dimensions in which the game will run. Once created the application runs.
    </p>

    {% highlight c++ %}
        #pragma once
        #include "ofMain.h"

        class testApp : public ofBaseApp{
            public:
            void setup();
            void update();
            void draw();

            void keyPressed(int key);
            void keyReleased(int key);
            void mouseMoved(int x, int y);
            void mouseDragged(int x, int y, int button);
            void mousePressed(int x, int y, int button);
            void mouseReleased(int x, int y, int button);
            void windowResized(int w, int h);
            void dragEvent(ofDragInfo dragInfo);
            void gotMessage(ofMessage msg);
        };
    {% endhighlight %}

    <p style="text-align:justify;">
    	In the testApp.h, we can see the basis for every application, and some key, mouse and windows events.<br />
    	The setup() method, sets all the elements of the application, this method will only be called at the beginning of the application. <br />
    	The update() method, updates all the elements of the application, this method will be called in every iteration of the application. <br />
    	The draw() method, draw all the elements of the application for the next frame that is going to show at screen, this method will be called after every update().
    </p>

    {% highlight c++ %}
        void testApp::setup(){
            ofEnableAlphaBlendhighlighting();
            // We set the application to run at 60 FPS.
            ofSetFrameRate(60);
            ...
        }
    {% endhighlight %}

    <p style="text-align:justify;">
    	At setup(), enable Alpha Blendhighlighting for rendhighlightering with transparency, here what oF does is call glEnable(GL_BLendhighlight); to rise OpenGL Blengin flag; and set the frame rate at 60FPS. Later we&#039;ll see what objects we set up, we must first create them.<br />
    	For the moment, the methods update() and draw() are empty. We could paint the window background in white or black (or whatever we wanted) using ofBackground(r,g,b);
    </p>

    <h2>
    	Car
    </h2>

    <p style="text-align:justify;">
    	Once we have an area to play, we need a toy. As we saw at class testApp, we create a new class with the 3 basic methods, setup(), update() and draw(), and some necessary attributes.
    </p>

    {% highlight c++ %}
        class Car{
            public:
            // We declare the basic methods for the class.
            Car();
            ~Car();

            void setup(float x, float y,float width, float height);
            void update(float dt);
            void draw();

            //GETERS &amp; SETERS//
            ...

            private:
            ofPoint   car_position_;
            float     car_rotation_;
            float     car_speed_;
            ofImage*  texture_;

            float     car_width_;
            float     car_height_;
        }
    {% endhighlight %}

    <p style="text-align:justify;">
    	We give values ​​to the setup() to configure the car. Setting the initial position (x,y) in the window and the texture size of the car (width,height), and the initial speed as 0 (we don&#039;t want the car run like crazy) and rotation to where it should be aimed initially in degrees (45&ordm; = diagonal up-right)..<br />
    	These data will be stored in the attributes of the class, in addition to the car&#039;s speed, the rotation and its texture.<br />
    	To modify all attributes also implement their getters and setters. With the only difference set_texture(), which will load the texture in memory.
    </p>

    {% highlight c++ %}
        void Car::set_texture(string path){
            texture_ = new ofImage();
            texture_-&gt;loadImage(path);
        }
    {% endhighlight %}

    <p style="text-align:justify;">
    	<a href="http://www.madowen.es/wp-content/uploads/2013/04/rotation.png" rel="" style="font-size:13px;text-align:justify;" target="" title=""><!--<img alt="rotation" class="alignright size-full wp-image-103" height="150" src="http://www.madowen.es/wp-content/uploads/2013/04/rotation.png" title="Rotation calc" width="155" />--></a> Now that we have set the car, we have to make it move. To calculate the position in the next frame, which must be the car when in motion, we will consider the current position, rotation, and the speed at which the car moves.<br />
    	First we calculate the direction in which we should move, using the cosine and sine of the car&#039;s rotation, it will provide us a unit vector (length = 1) which will be multiplied by the car&#039;s speed to get the distance advanced in the cycle. And we will add to the current position.
    </p>

    {% highlight c++ %}
        void Car::update(float dt){
            //cos() and sin() works in radians, so we have to transform degree-&gt;radian
            ofVec2f turn(cos(car_rotation_*PI/180),sin(car_rotation_*PI/180));
            ofPoint new_position = car_position_+turn*car_speed_*dt;

            car_position_.set(new_position);
        }
    {% endhighlight %}

    <p style="text-align:justify;">
    	The dt component, is the delta-time that is to correct the frame rate fluctuation.
    </p>

    <p style="text-align:justify;">
    	Once we know what position should be the car when moving, we have to draw it on the screen. To do this, we will move the object in the 2D world, translating to the position that we calculated before, and also rotating to aim according to need.
    </p>

    {% highlight c++ %}
        void Car::draw(){
            ofPushMatrix();
            ofTranslate(car_position_.x,car_position_.y);   //translate to correct position
            ofTranslate(car_width_/2,car_height_/2);        //move to the texture center
            ofRotate(car_rotation_,0,0,1);                  //rotate to correct orientation

            ofTranslate(-car_width_/2,-car_height_/2);      //return to correct position

            texture_-&gt;draw(0,0,car_width_,car_height_);     //draw the texture
            ofPopMatrix();
        }
    {% endhighlight %}
</div>
<div>
    <h2>
       Scenario
   </h2>

   <p style="text-align:justify;">
       What would be a racing game without a track where to compete? As each class in the class Scenario, we define the 3 basic methods, in addition to its attributes and functions needed for the class.
   </p>

   <pre>
    class Scenario{
        public:
        // Declaration of the basic methods for the scenario class.
        Scenario();
        ~Scenario();

        void setup();
        void update();
        void draw();

        //GETERS &amp; SETERS//
        ...

        private:
        ofImage**        texture_array_;
        int*             scenario_array_;
        vector  trees_vector_;

        int              scenario_width_;
        int              scenario_height_;

        float            tile_width_;
        float            tile_height_;
    };
    {% endhighlight %}

    <p style="text-align:justify;">
    	The texture array stores all different tiles to draw the track, the scenario array gives shape to the track with the indices from the texture array, also we define a vector to draw some trees. The width and height atributes are given by the number of tiles that fill the screen, it will have width*height tiles. And it is necessary set the width and height of the tiles.<br />
    	At the constructor, we reserve the space in memory for the instantiation of the class.
    </p>

    {% highlight c++ %}
        Scenario::Scenario(){
            texture_array_ = (ofImage**)malloc(7*sizeof(ofImage*));

            scenario_width_ = 8;
            scenario_height_ = 6;

            tile_width_ = ofGetWidth()/scenario_width_;
            tile_height_ = ofGetHeight()/scenario_height_;

            scenario_array_ = (int*)malloc(scenario_width_*scenario_height_*sizeof(int));
        }
    {% endhighlight %}

    <p style="text-align:justify;">
    	We allocate memory for 7 different tiles to draw the track, which will have 8*6 tiles. To draw the tiles so that they fill 100% of the screen, we divide the width of the screen by the number of tiles widthwise, and the same for the height. And finally allocate memory for 8*6 indices that will define the shape of the track.<br />
    	With the allocated memory, we load the textures and set the scenario in the setup() method.
    </p>

    {% highlight c++ %}
        void Scenario::setup(){

            // Just for loading purposes we print by console when we start loading the scenario.
            cout &lt;&lt; &quot;LOADING SCENARIO&quot; &lt;&lt; endhighlightl;

            cout &lt;&lt; &quot;SCENARIO: LOADING TILES IMAGES&quot; &lt;loadImage("track/grass.jpg");
            (*(texture_array_+1)) = new ofImage();
            (*(texture_array_+1))-&gt;loadImage("track/racing_curve_1.jpg");
            (*(texture_array_+2)) = new ofImage();
            (*(texture_array_+2))-&gt;loadImage("track/racing_curve_2.jpg");
            (*(texture_array_+3)) = new ofImage();
            (*(texture_array_+3))-&gt;loadImage("track/racing_curve_3.jpg");
            (*(texture_array_+4)) = new ofImage();
            (*(texture_array_+4))-&gt;loadImage("track/racing_curve_4.jpg");
            (*(texture_array_+5)) = new ofImage();
            (*(texture_array_+5))-&gt;loadImage("track/racing_road_vert.jpg");
            (*(texture_array_+6)) = new ofImage();
            (*(texture_array_+6))-&gt;loadImage("track/racing_road_hor.jpg");
            (*(texture_array_+7)) = new ofImage();
            (*(texture_array_+7))-&gt;loadImage("track/Tree.png");

            trees_vector_.push_back(ofPoint(187, 441));
            trees_vector_.push_back(ofPoint(221, 307));
            trees_vector_.push_back(ofPoint(563, 226));
            trees_vector_.push_back(ofPoint(509, 595));
            trees_vector_.push_back(ofPoint(903, 229));
            trees_vector_.push_back(ofPoint(755, 364));

            int temp[] =       {3, 6, 6, 6, 6, 6, 6, 4,
            5, 0, 0, 0, 0, 0, 3, 1,
            5, 0, 0, 3, 4, 0, 5, 0,
            5, 0, 3, 1, 5, 0, 2, 4,
            5, 0, 5, 0, 5, 0, 0, 5,
            2, 6, 1, 0, 2, 6, 6, 1};
            memcpy(scenario_array_,temp,sizeof(temp));
        }
    {% endhighlight %}

    <p style="text-align:justify;">
    	First we load the 7 different tiles for later drawing the scenario given by the scenario array. The sequence to load the images are used for indices that we use in the scenario array. Will place the trees in different positions of the screen. Initialize the scenario with 8*6 indices, according to the tiles we want to use to shape the track. (I&#039;m copying from temp to scenario_array because an int* can&#039;t be initialized as a list)
    </p>
    <p style="text-align:justify;">
    	<a href="http://www.madowen.es/wp-content/uploads/2013/04/SRC-Tiles.jpg"><!--<img alt="SRC - Tiles" class="aligncenter size-full wp-image-134" height="120" src="http://www.madowen.es/wp-content/uploads/2013/04/SRC-Tiles.jpg" width="959" />--></a>
    </p>
    <p style="text-align:justify;">
    	As the scenario represents the track where cars run, and it is an inanimate object, that will not change over time, the update() method will be empty.
    </p>

    {% highlight c++ %}
        void Scenario::draw(){
            // We set the color to draw at pure white.
            ofSetColor(255, 255, 255);

            for(int j = 0; j &lt; scenario_height_; j++){
                for(int i = 0; i &lt; scenario_width_; i++){
                    draw(i*tile_width_, j*tile_height_, tile_width_, tile_height_);
                }
            }

            for (std::vector::iterator it = trees_vector_.begin() ; it != trees_vector_.endhighlight(); ++it){
                ofPushMatrix();
                ofTranslate((*it).x, (*it).y);
                (*(texture_array_+7))-&gt;draw(0, 0, tile_width_/3, tile_height_/3);
                ofPopMatrix();
            }
        }
    {% endhighlight %}

    <p style="text-align:justify;">
    	In the draw() method has to loop the scenario array to get wich texture should be rendhighlighter, then draw it in the expected position. To draw the tree we iterate over the tree vector using an iterator to go through, iterators tendhighlight to be the most efficient way to search through a data container. For each position we do a push and popMatrix() so the call to ofTranslate() will not affect the posterior rendhighlighter in the game loop. By calling the ofTranslate(), we can pass as x and y arguments to ofImagen().draw() the values 0, 0.
    </p>
</div>
<div>
    <h2>
       Gameplay &amp; Fixes
   </h2>

   <p style="text-align:justify;">
       Now that we have ready the Scenario and Car classes, we should add them to the framework and initialize in the setup() method of the testApp class. We only need one instance of Scenario, but we could need more than one car if we want to play with friendhighlights, so they will be instantiated in an array of Car.<br />
       The initialization is quite simple, just call the setup() method of the scenario and the cars, passing to each car its initial position and size of the texture.
   </p>

   <pre>
    scenario_.setup();
    cars_[0].setup(120,37.5,35,20);
    cars_[0].set_texture("cars/Red.png");
    {% endhighlight %}

    <p style="text-align:justify;">
    	<a href="http://www.madowen.es/wp-content/uploads/2013/04/Red.png"><!--<img alt="Red" class="alignright size-full wp-image-136" height="98" src="http://www.madowen.es/wp-content/uploads/2013/04/Red.png" title="Red" width="127" />--></a>Right now, if we draw the scenario and cars in the method draw () and run the project, we have running our game, but we can not move the car. In order for the car move using the keyboard, we need to capture the event that happens when any key is pressed and when is released. The framework has already implemented these methods, but we have to tell them what to do when a button is pushed and released.
    </p>
    {% highlight c++ %}
        void testApp::keyPressed(int key){
            key_Pressed[key] = true;
        }

        void testApp::keyReleased(int key){
            key_Pressed[key] = false;
        }
    {% endhighlight %}
    <p style="text-align:justify;">
    	Quite simple, we store a boolean array wich key was pressed or not, using as index the int value of the key. Doing this way, we can press more than one key at the time.<br />
    	Now that we have the keys pressed or not, the game has to tell the car what to do on every frame for each key pressed.
    {% highlight c++ %}
            void testApp::update(){
                if (key_Pressed['w']) //Up
                    cars_[0].car_accelerate();
                if (key_Pressed['a']) //Left
                    cars_[0].car_turn_left();
                if (key_Pressed['d']) //Right
                    cars_[0].car_turn_right();
                if (key_Pressed['s']) //Down
                    cars_[0].car_reverse();
                if (!key_Pressed['w'] &amp;&amp; !key_Pressed['s']) //If the accelerating and reverse button are not pressed, the car breaks
                    cars_[0].car_break();

                cars_[0].update(dt);
            }
    {% endhighlight %}
    <p style="text-align:justify;">
    	Since for each frame we want the car to move, we will check in the update() if keys are pressed to move the car. Also if we want that when we do not press any key the car breaks, we have to check that the keys to accelerate and reverse are not pressed. The methods we use are pretty obviuos, accelerate, break and reverse increments or decrements the car speed atribute, and turn_right and turn_left increments or decrements the car rotation atribute. 
    </p>
    <p style="text-align:justify;">
    	Now that we can move our car across the scenario, problems arise. The most important thing to fix is the fact that we can get out of the screen. To fix that exists an OpenFramework method that returns true if a value is between a range, we use this method to check if our car will be outside the screen when we update its position, if it is true it won't move to the new position. This check has to be done every time that we want to move our car, so we add it at car's update() method.
    </p>
{%  highlight c++ %}
        void Car::update(float dt){

            ofVec2f turn(cos(car_rotation_*PI/180),sin(car_rotation_*PI/180)); //cos() and sin() works in radians
            ofPoint new_point = car_position_+turn*car_speed_*dt;

            //The position is only setted if we are on the limits of the scenario
            if (ofInRange(new_point.x,0, ofGetWidth()-car_width_) &amp;&amp; ofInRange(new_point.y,0,ofGetHeight()-car_height_))
                car_position_.set(new_point);
        }
{% endhighlight %}
    <p style="text-align:justify;">
    	From this point that we already have implemented the basic elements of game, simply must be added features like controlling the car goes through all the tiles forming the track, or when the car moves by the grass tiles, the speed is reduced, and even control the collisions between cars and trees.
    </p>
    <p>
    	<a href="http://www.madowen.es/wp-content/uploads/2013/04/SRG-Track.jpg" rel="" target="" title=""><!--<img alt="SRG - Track" class="aligncenter size-full wp-image-138" height="722" src="http://www.madowen.es/wp-content/uploads/2013/04/SRG-Track.jpg" title="SRG - Track" width="963" />--></a>
    </p>
    <p style="text-align:justify;">
        Here you have the source code. <a href="http://www.madowen.es/wp-content/uploads/2013/04/Simple-Race-Game-src.zip">SRG - src</a><br />
        Here you have the textures used and some extras. <a href="http://www.madowen.es/wp-content/uploads/2013/04/data.rar">SRG - Tiles</a>
    </p>
    </div>