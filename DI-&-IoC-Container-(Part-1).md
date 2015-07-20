[Watch on YouTube](https://www.youtube.com/watch?v=e2cLXc5Tmg0)

# Introduction
While using uFrame you may notice that a lot of things are already there when you need it. Like you have links to the Controllers, you have globally registered ViewModels, and you do not have to care where those come from. To keep track of all the instances and effectively creating ones when needed ,uFrame uses a so-called IoC Container or Dependency Injection Container, and its particular implementation called GameContainer. This guy basically manages all the types, and it is capable of giving you an instance of a type when needed, with all the dependencies injected.

So what is exactly dependency? I have a couple of printers here and as you can see printer cannot print properly without having his Head and PaperLoader set, so otherwise you will get null pointer exception. So somehow I need those guys injected. I could do it manually while constructing the printer, but I do not always have a copy of Head and PaperLoader with me. Also I want to be able to reuse Heads and PaperLoaders, because I don’t want to constantly create new ones. And this is our goal for today. So we need to construct Main class, and we need to have all its dependencies resolved. As you can see, the only dependency for the Main class is the Printer, or IPrinter Interface. I have two class which implement this interface, Printer2 and Printer1, and both of those are dependent on IHead and IPaperLoader.

I have one implementation of Head and one implementation for the PaperLoader. As usual my entry point is a Start() method of MonoBehaviour, in this case it’s container example. I have already created an instance of container, and I call the SetupContainer() method which we’re going to fill with some interesting things.

# Manually Resolving Single Instance Dependencies
Obviously we have to start creating our system with the most lightweight components like Head and PaperLoader because they do not have any dependencies. So let’s start with creating Head and PaperLoader. I can do container.RegisterInstance, and just create a new Head. But I want to be able to resolve it any time I need IHead, because really we want to decouple the Head implementation from everything else.

Now let’s do the same for the second type. And now I can manually resolve them so let’s test it. And I must provide a type which I have used to register the instance of one of those guys. So I will test it using IHead. Let’s see if it works.

So indeed, I got an instance of Head. Now let’s see what happens when we try to resolve IHead for the second time. So in my Head implementation, I'll add something to distinguish between different instances, and I'll print out this.GetHashCode().

So now instead of calling Debug.Log() I'll call Prepare(), and I'll do it twice. So let’s see what happens.

# Registering Multiple Unique Instances
As you can see I got the same instance every time I resolve IHead. So basically we can say that we have registered a single instance of Head, but sometimes that’s not the case. You may also want to create new Head every time you resolve it to create unique instances when needed, or you may have a couple of Heads in your application which you want to use in different classes. So let’s see how we can go about this. First, let’s learn how to construct unique instances every time we resolve it. So now instead of directly registering new instance of IHead, we will register a type mapping.

We can use Register() method and we provide a source type which is IHead, and a target type, which is Head. And basically here we say that “Hey container, every time we resolve IHead, create me a new instance of Head." Let’s see if it works.

So now we got two different hashes which means we got two different instances. The next use case is when you want to have a couple of Heads in your application, which means you do not want to create a unique instance every time you resolve IHead, but you want somehow to distinguish between a couple of instances.

# Identifiers
So we can use so called names or identifiers. Let’s register an instance. The source type will be IHead, and I’m going to create a new instance of Head, but I will also give it a name, for example, “Head1”. And I can also register the other instance of Head with the name “Head2”, and notice that you can use any other implementation of Head if you have one in your application. Now in the Resolve() method, I can use this name and get any instance I want. Let’s see if it works.

And in fact, it works. I got Head1 instance being resolved twice. And I got another instance with the name Head2. We'll continue to work with a setup where we have one single instance of Head for the entire application. Our next goal is to construct a Printer.

# Resolving Things Higher Up The Chain
Earlier we constructed two components which had no dependencies, but the Printer is slightly different, because it has dependencies, and in fact we want all the dependencies to be resolved automatically. One way to do this would be to use container.CreateInstance. We must provide a type and cast the returned object back to the type we want.

container.CreateInstance.(typeof(Printer1)) as Printer1);

Now we can cache this variable and print something.

Bingo - all the components were successfully resolved and our printer is fully functioning. Now let’s register our printer. container.RegisterInstance, and we will use IPrinterInterface as our source type, and pass in the printer instance we have. And now it’s time to construct the main class, and for now I will do it the same way I constructed our printer, and let’s see if it works.

Indeed, we got a printer setup and fully functioning. Let’s see what happens when we create an instance of some type. So container got some components already registered, and now it wants to create a printer. Well it goes to the target type, which is Printer1 and analyzes the constructor arguments. Here, it sees IHead, which is intact registered in the container, so container has no problem resolving this one, and it does the same for the IPaperLoader. This implies that to effectively construct a printer you have to have all the components registered before. Let’s see what happens if we remove IHead and register it sometime later after we create a printer.

We got a null pointer exception, which is the correct behavior, because container has no idea how to resolve IHead.

# Injection
The constructor arguments are not the only way to define dependencies. So let me fix this, and now let’s use [Injection] attribute to define dependency for the Main class. I’ll remove printer from the contractor and instead I’ll add [Inject] attribute, and notice that my dependency must be public. Now let’s test it, Bingo - again we got a printer fully functioning.

The other cool feature is that you can inject using the name. So let’s create a couple of printers. And we’ll register both printers under unique names. Now we can define which printer to use in the Main class by passing the name of the registered instance to the [Inject] attribute. Let’s see if it works.

So here we use Printer1 and this message in fact comes from the Printer itself, so we see what printer we use. Now let’s change the name to Printer2.

And now we can see we’re using Printer2. And this is how uFrame manages globally registered ViewModels, but we will talk about it later in the second part of the tutorial.

# Named Mappings
At this point we know how to use registered instances, with a certain name. But imagine that you want to create a new Printer each time you resolve IPrinter, but you want to create different implementation based on the name. So instead of directly creating an instance of a Printer, I will use so-called named mappings.

I will use Register() method for it. And I will give each mapping a name and a type to be created when this mapping is resolved. So this basically means that every time I’m injecting Printer2, it’s going to create a new instance of Printer2. But when I resolve Printer1, it’s going to create a new Instance of Printer1. Though they are all registered under IPrinter interface. Let’s see if it works.

Bingo - we got a new Printer1, with its own hash. Let me show you one final use case. Now instead of using container to create a main instance, I will create an instance myself. So I don’t have any constructor here and thats why I cannot pass any kind of printer. But I want to inject it somehow. Of course I could directly assign some printer here, but you see I don’t have a printer with me right now, and that’s a common use case. So what you can do about it is you can do container.Inject and pass in the instance of main class.

Container will go through your class, analyze all the properties, and find those that are injectable, and let’s see if it works.

Bingo - we got a printer successfully injected.

# Dependency Injection
At this point we know how to use container, but do we really know why we should use it? So let me explain to you. Forget for a second about the container and about all this stuff. Let’s answer some simple question: why do we use interfaces in our printer, instead of using the component implementations directly like Head and PaperLoader classes? The reason is simple: we want our printer to be able to use any Head and any PaperLoader. This makes our application flexible and configurable. The next simple question is why don’t we construct Heads and PaperLoaders inside of the printer. And the answer is we still want the low coupling. Because if we construct some Head inside the Printer, we will have to use a specific implementation, which automatically breaks our first concept.

Alright, let’s say we agree that Printer must use interfaces and it must not construct its own components. But unfortunately those guys won’t come from nowhere. There will be some point where those components should be constructed. And at first glance it’s really hard to define some sort of a class which is responsible for this. For example, we use the Main class which has a dependency which is Printer, so maybe we should construct Head and PaperLoader inside the Main class.

But in fact it will only add additionally coupling to our system because Main will be couple with specific implementations of Head and PaperLoader, and every time we will want to change the Head, we will have to go to our Main class and change it. And we can apply this logic to all the parent classes which use Main as their component, and so on and so on, until we recursively get to the entry point of our application to some sort of entry point class which has some Main function. Of course we could construct all the objects there, but unfortunately it’s hardly ever possible because in real world applications we construct objects everywhere, and we have to resolve their dependencies in time. This is where the idea of container becomes very natural, because we need some sort of  guy who will keep track of all the dependencies and create some objects for us resolving their dependencies and keeping track of all the objects created and using them to resolve other dependencies and so on and so on.

At this point we derive the idea of Dependency Injection, and now it’s time to learn how to apply this idea in the way that suits our application. Thankfully, we only have two ways to apply it. The first way is called IoC, or Inversion of Control. And the second one is called Service Location, or SL.

# Service Location
These two concepts each apply the idea of Dependency Injection in a slightly different way. Service Location means that we pass the container to the constructed object and the constructed object resolves its dependencies. Let me show you. Suppose we have a constructor in our Main class, and this constructor will accept container.

At this point, we can remove the [Inject] attribute, and just use container to resolve an instance of IPrinter. We then assign the resolved instance to the property which is our dependency. This is an example of Service Location, where the component essentially resolves the dependencies itself given the container.

# Inversion of Control
The other option is called Inversion of Control, and it basically says that components should never know about the container. Instead, it describes its dependencies using either constructor arguments, or the injection attribute, just like we did in the example. With such a setup, injection will happen automatically, but the container must be capable of doing this. In this implementation of Game Container, we have a method called CreateInstance(), which takes an instance and resolves its decencies based on the inject attribute and the constructor arguments. This means that this container is IoC container. So hopefully this explains a bit about the DI and IoC and I’ll see you in the [next part](https://github.com/InvertGames/uFrame/wiki/DI-&-IoC-Container-(Part-1)), where we will take a look at how uFrame uses its own container to manage different instances and different types.