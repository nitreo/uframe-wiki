[Watch on YouTube](https://www.youtube.com/watch?v=mVGyW0BKSTM)

# Introduction
A State Machine is a good way to express high level logic using the diagram. It can be used in a lot of different ways. For example, AI logic. uFrame has built in support for State Machines. Let’s see how it works.

In this example I have two Elements, SimpleAI and Victim. Victim has a [Scene Property](https://github.com/InvertGames/uFrame/wiki/Scene-Properties). SimpleAI has a Scene Property called DistanceToVictim. Both Scene Properties are already implemented. I have a link to the Victim and Relax command. 

For this SimpleAI we’re going to create a state system based on the DistanceToVictim. To do this, I first have to create a State Machine Node. I can go inside my Element, right-click, and select Add State Machine. I’m going to name this AIStateMachine.

Now I have to introduce a property on my SimpleAI and link it to the State Machine. As you can see now the type of the property is AIStateMachine.

# States and Transitions
Double-click on the State Machine and you can start designing your states. In my case, I’m going to introduce four states: Relaxed, Alarmed, AttackPlayer, and ChasePlayer.

Now when we have states defined, we’re going to define a few transitions. If I drag the output connector of my State Machine to some state, this is going to be my default state. So whenever my Enemy appears, it’s going to be in a Relaxed state.

Whenever I see the player or the player is in spot range, I’m going to chase him. So I’m going to add a transition here.

Now I can select it here and drag it to ChasePlayer. While I’m chasing the player I’m going to want to be able to attack it. So I’ll add another transition called CanAttackPlayer, and connect it with the AttackPlayer state.

When we’re attacking the player, suddenly you may run away, so you won’t be able to attack, so I’ll add CantAttackPlayer transition. Add it to the AttackPlayer state and drag it to ChasePlayer state.

At some point, the player could run too far away, so I’ll add another transition called PlayerLost, add it to the ChasePlayer state, and connect it to Alarmed state, because I want the AI to stay Alarmed for a little while.

Finally, at some point, I may say “OK, I’m going to relax because the player is too far away”, so I’ll introduce a Relax transition. Add it to the Alarmed state, and connect the transition to the Relaxed state.

# Triggers
Now we have states and transitions defined. So how do we actually trigger the transition? Well for this, you can use either a command, like this command Relax here, and drag it to Relax transition, and now whenever I’m in the Alarmed state and the Relax command is executed I’m going to transition to Relaxed state.

For more complex logic you can introduce computed properties. Let’s say I have a computed boolean property called PlayerInAttackRange. This can be easily dragged to CanAttackPlayer, so that whenever the player is in the attack range, it will compute to “true” and we can attack him. This will be dependent on DistanceToVictim.

Using the same logic I can define another computed property called PlayerOutOfAttackRange. This will be dependent on the previous Computed Property, because it’s just the inverse of PlayerInAttackRange. And I’m going to drag it to CantAttackPlayer.

Also, I’ll add something like PlayerInSpotRange. This one again is going to be dependent on DistanceToVictim, and whenever the player is in the spot range, it means the player has been spotted.

Finally, the player can be out of spot range. This is again just the opposite of the previous expression. I’m going to drag it to PlayerLost.

Let’s also fix the issue here with the Alarmed state, because whenever the player is spotted again, I’m going to transition back to ChasePlayer. And here you can see how you can reuse different transitions for different states.

Back to our computeds. Those are all boolean types and that’s why they can serve as so-called “triggers” for State Machine transitions. In this case, whenever the value is set to true, it’s going to invoke the corresponding transition or transitions. Now that we have our logic implemented on the diagram, let’s also implement it in our code.

I’ll go to my ViewModel and paste my computeds here. As you can see, the attack range is considered to be less than 3, and the spot range is considered to be less than 10. The other two Computed Properties will just be the inverse of the corresponding properties, and I can rely on those because I have those dependencies defined in my diagram.

In my scene, I already have an enemy prepared with a link to the player. If I hit Play, you can see that once I start moving this player, the values of the enemy ViewModel will change in the inspector. When debugging your states, you can use either this state property here in the view inspector, or you can dock the diagram window somewhere, choose your state machine diagram, and once you have an object selected that interacts with the State Machine, you’ll be able to track all the transitions and all the states.

# States in Action
It’s all nice and fancy, but it’s not much of use if we can’t rely on those states in the View or in the Controller. Let’s start with the View. If you have a View connected to your Element and you open the Add Binding window, under the State Machine Property Binding, you’ll find StateChanged, which stands for the State property on your Element. I’m going to add this to my View, hit Save and Compile, and open my View.

Here I’ve got a bunch of stuff generated. First of all, I have this StateChanged binding. But also for the convenience you have all the handlers for different states generated.

Here I have a _sprite property that I’ve cached in the Awake() method, and I’m going to use it to provide some sort of visual feedback. I’m simply going to change the color of my sprite.

Now if I go to my scene and start dragging the player around, you can see that the enemy changes color. And I can execute Relax to make him green.

uFrame also allows you to define any additional logic related to your State Machine. Let me go to my SimpleAIController. Here, whenever I initialize SimpleAI ViewModel, I can grab its StateProperty and subscribe to it. I’ll get my new state here, and I’ll execute a handler.

Inside of it, I’m going to figure out if my state is alarmed. If it is, I’m going to create a timer. Let’s set it to three seconds.

Whenever the timer finishes, I’m going to execute a command on my SimpleAI called Relax(). So now, if my AI enters Alarmed state, it’s going to wait a bit and start relaxing.