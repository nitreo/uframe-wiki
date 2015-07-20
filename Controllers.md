[Watch on YouTube](https://www.youtube.com/watch?v=4Xs_u5lJiyg)

# Introduction
Even though Controllers are not part of the original MVVM pattern, it's used in conjunction with the ViewModel to encapsulate logic. On top of this diagram, you can see the original concept of MVVM. In uFrame however the logic is not stored on the ViewModel. Instead, whenever you execute a command it’s delegated to the corresponding Controller. Game logic can be quite heavy, but your ViewModel keeps being very lightweight since all the heavy logic is in the Controller.

# Controllers
Any Controller starts with a special method called InitializeElementName(). Whenever you create a ViewModel using Controller.CreateElementName(), in this case CreatebGameRoot(), this method is invoked, and you can define any initialization logic here.

In this diagram I have a BuldlingSystem which exposes the globally registered instance of BuildingManager. My bGameRoot is stored inside the BuildingGameSystem, which is connected with the BuildingSystem. That’s why in the Controller, I have access to the globally registered instance of the BuildingManager.

Additionally Controllers can take advantage of decency injection, which is a topic for another tutorial. But the most important ability of any Controller is command handling. This will be discussed in detail later in this tutorial.

# Commands
When designing an Element, you can define commands. Commands are tightly connected with the Controllers. Intuitively commands serve as a trigger for some portion of logic defined in your Controller. If your logic requires an input, you can pass an object of a certain class. Just click left of your command name and pick out your type. In my case, it’s going to be boolean.

# Command Handlers
When you define a command on your Element, a command handler is generated in the Controller base. You can modify it by going to the corresponding Controller and overriding the method with the same name as your command. In my case I have two commands, Command1() and Command2(), so let me go ahead and override those.

Both handlers receiver a sender. This is a ViewModel on which you have executed a command. Additionally, the second command receives a boolean argument. You can be very creative inside of those handlers and modify the ViewModel instance and any other environment. This is where you store the core logic of your game.

# Executing Commands
Executing commands is very simple, however the process can be slightly different in different parts of the code. Here’s a little table which will help us, and let’s start with the View. Here I’ve got a View called JustAnElementView which works with the ViewModel of JustAnElement. If I open up the code and just paste in some dummy method, my auto-complete will show me that I’ve got methods called ExecuteCommand1(), and it receives no arguments. And I also got ExecuteCommand2(), which receives a boolean. Because JustAnElementView works with the JustAnElement ViewModel, uFrame generated some helpers for us. And you can use those to execute commands on the linked ViewModel instance.

But sometimes you want to execute commands on another instance which is not linked on this View. So let’s say I’ve got a TurretViewModel from nowhere. And I want to execute the Kill() command on this turret. So what I do is this.ExecuteCommand(turret.Kill) command, with a possible argument there, which is null here. A command which takes an argument would be invoked as this.ExecuteCommand(turret.Kill, argument).

Sometimes you will want to execute commands right from the Controller. So let’s go to JustAnElementController, and here in the Initialize() method, I can do this.ExecuteCommand, and I can provide a command here (let’s say Command2), and I can provide a possible object argument. Let’s say it’s false.

    this.ExecuteCommand(justAnElement.Command2, false);

And once I do so, this handler will be invoked because this JustAnElement instance is linked to this Controller. And this is how you execute any command.

So to sum up, commands just serve to handle the logic, and this is a good place to change some variables on your ViewModel or communicate with the environment.