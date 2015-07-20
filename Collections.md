


# Introduction
Remember that [one of the core concepts](https://github.com/InvertGames/uFrame/wiki/Core-Concepts) in the uFrame MVVM design pattern is the separation of our object data and how that data gets represented in our games. An important piece to this idea is that because Views "live in the game" and ViewModels and Controllers "live in the data sphere", they don't really know or care about each other. Controllers do NOT know about Views and Views do NOT know about Controllers.

You can think of a Collection as a `IList<nodeType>`. In fact, collections as well as properties and commands are implemented in a more advanced way.

![](http://i.imgur.com/0MShghM.png)

# Scene First Collections
See [Instantiation Scenarios and Methods](https://github.com/InvertGames/uFrame/wiki/Instantiation-Scenarios-and-Methods)

# Creating Your ViewModel
Keeping that in mind, the first thing we'd then normally want to take care of is creating and adding our ViewModel to our collection. The logic for this will usually go in our Controller. For example, let's say we have an EnemyManager with a collection of Enemies and a CreateEnemy() command.

![](http://i.imgur.com/oyYtP7A.png)

We can call CreateEnemy() any time we want to add an enemy to the collection, and override the command in the EnemyManagerController:

    public override void CreateEnemy (EnemyManagerViewModel enemyManager)
    {
        base.CreateEnemy (enemyManager);

        var enemy = new EnemyViewModel(EnemyController); //create the ViewModel using the corresponding Controller
        enemyManager.Enemies.Add(enemy); //add the ViewModel to our collection
    }

# Instantiating Your View
We then later represent that change in our game by instantiating a game object with a View that points to the correct ViewModel. In your EnemyManagerView you can add a special binding called CreateEnemyView where you'll instantiate your EnemyView.

![](http://i.imgur.com/TUp0ShV.png)

    /// This binding will add or remove views based on an element/viewmodel collection.
    public override ViewBase CreateEnemyView(EnemyViewModel item) {

        // here you can specify a prefab to use, a ViewModel to associate with, and a starting position
        // the prefab must be located in a Resources folder and have an EnemyView attached
        var v = InstantiateView(item);
        return v;
    }

Here your prefab **must**:

* Have a name that matches the name of your element, for example "Enemy"
* Be located in a Resources folder. uFrame will try to find a prefab with a name that matches your element
* Have a View attached with the name "ElementNameView", for example "EnemyView"

Or you could specify a specific prefab to use.

    /// This binding will add or remove views based on an element/viewmodel collection.
    public override ViewBase CreateEnemyView(EnemyViewModel item) {

        // here you can specify a prefab to use, a ViewModel to associate with, and a starting position
        // the prefab must be located in a Resources folder and have an EnemyView attached
        var v = InstantiateView("PrefabName", item);
        return v;
    }

or...

    /// This binding will add or remove views based on an element/viewmodel collection.
    public override ViewBase CreateEnemyView(EnemyViewModel item) {

        // here you can specify a prefab to use, a ViewModel to associate with, and a starting position
        // the prefab must be located in a Resources folder and have an EnemyView attached
        var prefab = (GameObject)Resources.Load("SomeFolderWithinResources/SomeOtherFolderIfNeeded/PlayerViewPrefab");
        var v = InstantiateView(prefab, item);
        return v;
    }

Here your prefab **must**:
* Have an EnemyView attached

On your EnemyManagerView you can then specify a parent for your game objects to spawn under.

![](http://i.imgur.com/KWVUZWJ.png)

# Additional Resources
* [Instantiation Scenarios and Methods](https://github.com/InvertGames/uFrame/wiki/Instantiation-Scenarios-and-Methods)
* [Aahz's RTS Example Project](https://github.com/InvertGames/uFrame/wiki/Examples)