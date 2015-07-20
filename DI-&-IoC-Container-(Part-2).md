[Watch on YouTube](https://www.youtube.com/watch?v=nkvwI7PnD6o)

# Example Setup

We have a scenemanager, a subsystem and couple of Elements. Notice that ElementB is a globally registered element with a slightly adjusted name : TheBInstance

# GameManager and Referencing the Container

So the show starts in the GameManager script. You can find a property called Container. As you can see, it is constructed right here, and it instantly gets 2 types registered which are LoadLevelViewModel and EventAggregator.

LoadLevelViewModel is a specific type for managing level loading, and EventAggregator is responsible for managing events.

There is also another method invoked here, which gets the container and an Instance of gamemanager. But there is not much magic happing there. We just register a couple of essential instances which uFrame needs.

So basically you can refer to container using GameManager.Container. Notice though, that it is not really a good practice, because GameManager is Unity Specific and you want your own classes to be as portable as possible and not rely on Unity-Specific API,

# Scene Manager Setup() Method

In general, once GameManager is initialized it passes control to the currently active SceneManager
and it's Setup method gets invoked. Notice how we call Container.Inject(sceneManager) to resolve all the dependencies for the current SceneManager.

The general loading procedure is not finished at this point, but we are going to focus on this setup method, where the main action is happening. In my scene I have GameManager and it's Start sceneManger is set to TestSceneManger.

So obviously, this one will be loaded and initialized during the loading procedure. Let's take a look at what is happening in the setup method of TestSceneManager

Well it turns out that no magic is happening here too. We just register certain ViewModels, Controller, and other types, sometimes giving it a name.

After we registered all the instances and mappings, we call Container.InjectAll()

At this point Container internally goes through all the instances which were registered, and resolves their dependencies which are defined using Inject attribute.

There are a couple of interesing things here. First we have those properties for Controllers and even though we basically construct a controller here, we still have the Inject attribute around.

Well when setting up the SceneManager, there can be 2 situations:

* If controller already exists in the Container, it will get injected into this sceneManager when GameManager called Container.Inject(sceneManger)

* If it's not the case, then SceneManager will construct this controller here, in the property.

And that's it. We have all the types with all the dependencies resolved.