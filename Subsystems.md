[Watch on YouTube](https://www.youtube.com/watch?v=L9JIVFR-AFc)

# Introduction
When designing your game, it’s a good practice to separate different scripts into systems. You may AimingSystem, AISystem, MovementSystem, and so on. A system may encapsulate a lot of things, but exposes a simple interface for you and your game. The same applies for uFrame. It gives you an ability to separate different parts of your game in the diagram so you can focus on one task at a time. in uFrame 1.5 Subsystems can also serve to define Global Instances of Elements. This will be also covered in this tutorial, but now let’s start with something simple. This is my main diagram. Now it’s cluttered with tons of different Subsystems. This is because I don’t use awesome workflow features yet which come with uFrame 1.5.

![](http://i.imgur.com/Osi5AtW.png)

Here I can right-click and select Add Subsystem. Alternatively I can use Ctrl-Alt-U shortcut. I’ll name this one BuildingSystem.

![](http://i.imgur.com/LB5Uiig.png)

Inside of any Subsystem Elements are stored. I’m going to create BuildingManager. Also I’ll create a Map and a Building. For now I’m not interested much in the properties, I’m just going to make it look pretty.

![](http://i.imgur.com/DFAYLL4.png)

Back in the main diagram, I can now create any Scene Manager I want and link the Subsystem to this newly created Scene Manager. This will tell WhateverSceneManager to work with this BuildingSystem. It ensures bootstrapping all the necessary classes like Controllers and Registered ViewModels when the scene loads.

![](http://i.imgur.com/fAzuPvo.png)

The Scene Manager only allows one linked Subsystem at a time. But we’re probably going to have something else in our game rather than just Building. So what I can do here is I can create another Subsystem. I’ll call it BuildingGameSystem.

![](http://i.imgur.com/dUT88Gv.png)

Inside of it I can define any classes I want related to this particular game.

![](http://i.imgur.com/GV4ffkB.png)

And now instead of directly linking the BuildingSystem to WhateverSceneManager, I can link it to BuildingGameSystem to make it part of it. And now I can link BuildingGameSystem to WhateverSceneManager.

![](http://i.imgur.com/Pz970Yw.png)

The cool thing about this approach is that unlike Scene Managers, Subsystems allow you to connect as many Subsystems as you want. So if I will ever need a ScoreSystem in my BuildingGame, I can just link it.

![](http://i.imgur.com/nxmxpXZ.png)

# External Subsystems
If you work in a team you’ll probably appreciate a new feature in 1.5 which allows you to define separated diagrams for draft Subsystems, Elements, and State machines.

![](http://i.imgur.com/zLqH13O.png)

[(Animated GIF)](http://i.imgur.com/YglJkPD.gifv)

So imagine another guy is working on a subsystem and you do not want to accidentally modify it. What we can do is we can go to our Scene Flow, click this guy here, and choose Create External Subsystem Graph. Then you can choose it here from the menu.

![](http://i.imgur.com/0z26ayY.png)

[(Animated GIF)](http://i.imgur.com/qEPLAKj.gifv)

This will bring you inside this Subsystem and you can only work here. I’m going to rename it to something sensible like SocialSystem. And this is probably going to have an Element called SocialManager.

![](http://i.imgur.com/nUqkD6S.png)

Now in the MainDiagram, I can refer to this Subsystem by right-clicking on the digram, selecting Show Item -> SocialSystem. You cannot modify or go inside of it, so it’s completely encapsulated. However, you can link this Subsystem to any other Subsystem.

![](http://i.imgur.com/OZoW2CE.png)
![](http://i.imgur.com/ivVNHOn.png)

# Registered Instances
In uFrame 1.4 there were a lot of issues with having global ViewModel instances around different scenes and Subsystems. uFrame 1.5 allows you to do the following. In my BuildingSystem, I have this BuildingManager, and I want to have it as a shared Global Instance around my game. So I can expand this BuildingSystem, click this ‘+’ button, and pick out my BuildingManager.

![](http://i.imgur.com/0S4uhYm.png)

Now if I hit Save and Compile and let the code get generated, in my BGameRootController, I’ll have a reference to BuildingManager instance. And this applies to all the Controllers that are in a Subsystem which you've connected this BuildingSystem to.

![](http://i.imgur.com/TIYXU7v.png)

Also if I define a View for my BuildingManager and hit Save and Compile, in my hierarchy I can create a new game object with BuildingManagerView, and here you have this little button “Use Registered BuildingManager Instance. Click that and this View will use a Global Registered Instance (ViewModel) of BuildingManager.

![](http://i.imgur.com/IiGVYiW.png)

A good note here is how this entire concept plays with inheritance. Sometimes it’s useful to have a base manager which can be abstract and define some simple interface, and then create other systems with some concrete implementations of this manager. Finally you can use different systems and different implementations of the same manager for different scene managers. A good example of this can be game rules. So let me remove this link for a second and create another Subsystem.

![](http://i.imgur.com/PMkf7rT.png)

I’ll call it ModifiedBuildingSystem. I’ll connect the BuildingSystem to this one, and I’ll connect ModifiedBuildingSystem to the BuildingGameSystem.

![](http://i.imgur.com/ihRcCb5.png)

Inside of ModifiedBuildingSystem I’ll create a new Element called ModifiedBuildingManager. I’ll make it inherit from BuildManager.

![](http://i.imgur.com/chvALDF.png)

![](http://i.imgur.com/BVdScxX.png)

Finally I’ll expand my ModifiedBuildingSystem and add ModifiedBuildingManager as a Registered Instance. But in this case I’ll change the registered name so that it matches the name of the registered instance in the BuildingSystem, and now ModifiedBuildingManager will override the simple BuildingManager.

![](http://i.imgur.com/9pbZFwz.png)

WhateverSceneManager will now receive this ModifiedBuildingManager implementation of BuildingManager. However, I can still connect my BuildingSystem to another Scene Manager, in which case StateMachineTutManager will receive a Registered Instance of the base BuildingManager implementation.

![](http://i.imgur.com/G25rkwr.png)