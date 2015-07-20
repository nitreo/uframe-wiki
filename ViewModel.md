A _ViewModel_ serves as interface between the visual entities and logic of the game. These entities include properties (Score, Ammo, etc.), collections (Players, Environments etc.), and commands (PlayerHit, Upgrade etc.).

A Controller works with a ViewModel to initialize and modify data when a command is invoked and/or interaction occurs between an event and the user. 

In uFrame, both the Controller and the ViewModel are "portable" parts of a game. This means taking them outside of Unity and putting them into a separate environment, such as a terminal application or web server, will never be an issue. In fact, this is essential to providing proper support for the extended features of uFrame and is enforced by the Element Designer (which limit the types that are available). This should always be taken into consideration when developing or extending Controllers or ViewModels. 

## Diagram

![](http://i.imgur.com/oVunJef.png)

 - ViewModels: The objects, including properties and available commands

- Controllers: The rules, where you implement logic of commands

- Views: The visual/auditory representation of your objects in a game engine environment 

## ViewModel Manager

By default uFrame keeps up with viewmodels for us. It maintains a manager for each type of viewmodel you create. Read more on [ViewModel Managers](ViewModelManager).

To access these managers, you can inject them into controllers and services using the following code.

```
[Inject] public IViewModelManager<MyViewModel> MyViewModelsManager { get; set; }
```