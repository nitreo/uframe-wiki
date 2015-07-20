## Overview
So when you are working with uFrame you have 2 main scenarios:

### Scene First
This is when you have a prefab with the view component applied within the scene already. Generally in this case you would be wanting to tick the "Initialize View Model" box as this will tell uFrame that you are going to want the View to instantiate the ViewModel's property values. This can be done within the InitializeViewModel method within the view.

```
    protected override void InitializeViewModel(ViewModel viewModel)
    {
        base.InitializeViewModel(viewModel);
        // Put your viewModel.Stuff = Something; in here
    }
```

Scene first is often not the favored way of doing things as it is not entirely data driven and it puts the view in charge of the view model which is a bit back to front. However it is often the simpler way of getting up and running when, as you do not need to worry about spawn points or other concerns, you can just drop your prefab in the scene and off you go.

### Code First
This is a slightly more complicated approach as you are creating the view model, and instantiating the view then putting it into the view. This is data driven and gives you more control over how the view model should be controlled.

Generally for this approach the SceneManager provides and entry point for adding things to the scene, you are free to add as much complexity as you wish here, i.e having custom objects in the scene which should house certain instances, or doing different view creations for different configurations of view models etc.

The most common route is to add the setup logic within the SceneManager classes `OnLoaded` method, as this indicates the scene and all dependencies are bound and loaded, so you can then start adding things to the scene as needed. Most of the time you can just use the controller to create your viewmodel, such as `var viewModel = MyViewModelController.CreateMyViewModel()` then you can work out how to create your view.

#### When using prefab views
When using prefab views uFrame will try to locate a prefab for you, so assuming your viewmodel was called PlayerViewModel and you were to make the view via `InstantiateView(myViewModel)` uFrame would try to find a prefab called "Player" in the resources folder. You can enforce a type of view by doing the following:

```
var prefab = (GameObject) Resources.Load("SomeFolderWithinResources/SomeOtherFolderIfNeeded/PlayerViewPrefab");
var specificView = InstantiateView(prefab, viewModel);
```

This will then generally create a view with the viewmodel you have specified, you can easily change the prefabs here based upon viewmodel information, a common example would be if you were to have different enemy types. You could have an enum for the enemy type within your viewmodel and then when you go to instantiate the view you could pick a different prefab based upon the value of the enum i.e. SoldierPrefab, WraithPrefab, VillagerPrefab etc.

Another thing to mention is that you can also instantiate multiple views for the same viewmodel here, so if you were to have an avatar view (the player model you move around) and also a player HUD view (the players health, stamina, magic etc) you could instantiate both views from the same view model here and just put them in the view as you wish. This is another reason why code first is preferred as it is easier to support more complex view concerns and multiple views for a single viewmodel, which is harder to do via scene first without having to do some more complex model resolving.

#### When not using prefab views
This is a lot simpler when you are not using prefabs as you can just call `InstantiateView` and it will just create a default instance of that view in the scene, then you can manually do whatever you want to the views game object, such as attaching child game objects or however you express your view concerns.