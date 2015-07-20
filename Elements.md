# Basics

## Introduction

### What an element is?

Elements represent the [ViewModel](ViewModel) in the [MVVM pattern](MVVMPattern). In the game world it can be a player, weapon, menu screen or any other game entity. Elements hold only the data associated with the game’s entity. For example, Player element could contain information about player properties like health, running speed, obtained weapons or actions; Shoot, TakeDamage or Die.

![](https://dl.dropboxusercontent.com/u/75445779/uFrame_wiki/Screenshot_90.png)

Technically, an Element is a node defined in the graph that contains informations used to create ViewModel and Controller classes.

![](https://dl.dropboxusercontent.com/u/75445779/uFrame_wiki/node_to_code_generation.png)

Elements in itself doesn’t contain any logic. They are also completely game engine independent meaning that they have no knowledge of Unity related elements like game objects or components.

Each element can be represented in the game world through a [View](Views). View is a MonoBehaviour that will take the data from an element and represent it in the game. For example, it’ll play the die animation when the player’s health reaches zero.

### What is it for?

You can use it to model the game world entities with their attributes and actions even before making any Unity related stuff. They’ll be later represented visually in the game.

## Element Attributes

#### Properties

Properties are like C# class properties but more powerfull. When changed, they can invoke [State Machine](ReactiveStateMachines) transitions, [Computed Properties](ComputedProperties) and any other method that is subscribed to them (read more in the [Code section](#code)).

You can select the property type from a list:

![](https://dl.dropboxusercontent.com/u/75445779/uFrame_wiki/Screenshot_93.png)

or you can drag another element, [Type Reference](TypeReferences), [Enum](Enums) or [StateMachine](StateMachine) to use its type:

![](https://dl.dropboxusercontent.com/u/75445779/uFrame_wiki/Screenshot_94.png)

Linking a property to a view can make it a [Scene Property](SceneProperties).

#### Collections

Collections can store multiple elements of the same type. For example, you can store references to multiple menu screens that can be displayed/hidden on demand.

![](https://dl.dropboxusercontent.com/u/75445779/uFrame_wiki/Screenshot_95.png)

You can subscribe to changes in the collections and execute custom actions when an element is added/removed from the list.

[subscription example]

#### Commands

Commands allow to change the state of the ViewModel that the Element represents. If player gets damage, the TakeDamage command can be executed to descrease the player’s health. Usually, the ViewModel data should not be changed directly but only through the use of the defined commands. There are however use cases where VMs data can be changed directly from the View with [Scene Properties](SceneProperties) or [Services](Services).

[link to an example]

The actual implementation of the command is not stored inside the ViewModel but inside its [Controller](Controllers). When a ViewModel’s command gets called, it executes the command’s implementation defined in the controller. The controller then changes the ViewModel data what next triggers [bindings](Bindings) defined in the View.

![](https://dl.dropboxusercontent.com/u/75445779/uFrame_wiki/uFrame_MVVM_flow.png)

Read more about it in the [Advanced section](#Advanced).

### Nodes you can you link to the Element

I you make a connection from a property to another node then the node’s type will become type of the property. The same is true for Collections.

You can think of a Collection as a `IList<nodeType>`. In fact, collections as well as properties and commands are implemented in a more advanced way. You can read about it [here](some page with detailed explanation how properties, collections and commands are implemented).

If you make a connection from a Command to another node, then the command will accept a parameter of a type of that node. 

### Context Menu

You have separate context menu for the node header and its attributes. Most of the options is self-explanatory. Other are explained below:

* **Hide**

    Allows to hide nodes that you don’t want to be on the diagram. You can restore node by clicking on the canvas and selecting node from the Show Item -> <Graph Name> menu. It is important to note, that Hide is not the same as Remove, as Node still present in the system and you can show it in any graph, which supports corresponding node type.   

## Code

### How elements are represented in the code?
After creating an Element, setting its properties and recompiling, uFrame will create two editable files: {ElementName}ViewModel and {ElementName}Controller.

Each of those files is empty by default and is intended to by filled with implementation by the user. All the attributes specified in the diagram and the MVVM code are specified in their base classes.

Read more about [ViewModelBase](ViewModelBase) and [ControllerBase](ControllerBase) base classes.

### ViewModel

[ViewModel](ViewModel) is a class where all the data associated with a game entity is kept. You can also implement there [Computed Properties](Computed Properties), initialize [State Machines](Reactive State Machines), implement your own serialization methods  and define any other methods you need.

### Controller

[Controller](Controller) is created along with the ViewModel and it’s responsible for implementing logic behind the ViewModel. Any command that is specified in the graph Element will be implemented here.

[Add example code]

# Advanced

## Inheritance

You can connect two elements together to make one of the inherit all attributes from the other one. In the example below, SettingsScreen Element will contain the IsActive property and Close command of its parent SubScreen.

![](https://dl.dropboxusercontent.com/u/75445779/uFrame_wiki/Screenshot_97.png)

## Code
Generated code (behind the scenes).
All elements defined in the designer will be converted to C# code after hitting the Save & Compile button. uFrame will split the code into editable files (classes) which can be edited by the user and non-editable which should not be edited. The non-editable files will be regenerated after each assembly reload (recompilation).

For more info see the topic [uFrame File Structure](uFrame File Structure).

# Contribute

If you would like to contribute to this page, please create an issue and describe changes you would like to make.