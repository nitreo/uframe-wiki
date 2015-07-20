# Downloading and Installing
You can use either the official release on the asset store, [or download the most up-to-date version here](https://github.com/InvertGames/uFrame) and replace the uFrameComplete/uFrame folder that's in your current project.

#Initial Setup
We'll create a new uFrame project by going to Window -> uFrame Designer. We don't have a project defined yet, so uFrame designer will allow us to create one. Give it a name, hit "Create Project", and a new project and diagram will be created. Each project can use multiple diagrams, and your diagrams contain the layout for all the elements you'll be mapping out in your game. 

In your project hierarchy you'll see you've got a folder generated with a name that matches your project. uFrame will store your uFrame project file, diagram files, and all your generated code here. Before we actually start, let's rename our graph to something more memorable, like "MainDiagram".

#Game Managers and Scene Managers
Each one of your scenes will contain a **Game Manager** and a **Scene Manager**. The Game Manager has the longest life-cycle of all the uFrame classes and will remain throughout the entire lifecycle of your game.  It uses the Unity "DontDestroyOnLoad" functionality allowing it to persist from scene to scene. The Game Manager's main responsibility is to do initial setup of your scene's Scene Manager for uFrame.

The Scene Manager is in charge of handling each scene and is managed and accessible via the Game Manager. There should only be one available at a time. Its main responsibility is to setup the container and load anything needed to properly run your game. This could include registering ViewModel's in the Container, instantiating Views, or instantiating and initializing Controllers.

#Preparing the Scene
Right-click in the uFrame Designer window and create a Scene Manager. Give it a name that corresponds to the type of scene it will be handling, for example "MainMenuSceneManager" or "LevelSceneManager". Here is where you'll also create the main [systems](https://github.com/InvertGames/uFrame/wiki/Subsystems) for your game, for example "AudioSystem", "AISystem", or "MovementSystem", and the Scene Manager will be in charge of handling the setup for each of these systems. Note that each Scene Manager only has one system attach point, but you can chain them together to add additional functionality to your scenes.

![](http://i.imgur.com/PLOPUU8.png)

Once you've created your systems you can link them to your Scene Manager by clicking and dragging the connectors. When you're done connecting everything together, hit "Save and Compile".

![](http://i.imgur.com/O0s7Sus.png) ![](http://i.imgur.com/AQmbno1.png)

If you look inside your project folder you'll see that uFrame generated a few classes for us. Designer files are maintained and generated by uFrame; you're not supposed to modify those. However, you're working code is stored inside different folders. For now we've just got code generated for the LevelSceneManager.

![](http://i.imgur.com/LVKGeIo.png)

Now we'll prepare the scene to work with the LevelSceneManager. To do this, we create a few empty game objects. Call one "_GameManager" and the other "_SceneManager". Add the **GameManager** component to your _GameManager object, and the LevelSceneManager component to your _SceneManager object. Finally, we're going to tell our GameManager to work with this Scene Manager by linking it in the inspector. Save the scene and it's now ready to work with uFrame.

![](http://i.imgur.com/c7veAuR.png)
![](http://i.imgur.com/MJMr1x4.png)
![](http://i.imgur.com/MG47sZL.png)