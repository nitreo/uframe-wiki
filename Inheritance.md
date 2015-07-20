[Watch on YouTube](https://www.youtube.com/watch?v=gDtFChc5SQs)

# Inheritance
Inheritance is a great way to make your classes maintainable and extensible. uFrame provides a convenient way to express inheritance of Elements right in the diagram. Let’s say we’ve got ElementA with some properties, and we’ve got ElementB. We want this ElementB to be derived from ElementA. For this, I just drag the output connector of ElementA to the input connector of ElementB.

Now if I Save and Compile and look at the code, we’ll see that now ElementBControllerBase is actually derived from ElementAController. ElementBViewModelBase is also derived from ElementAViewModel. So whenever you express inheritance between two elements, ViewModels and Controllers are derived automatically. However, Views are not. So if we drop in a few Views for ElementA and ElementB, we will see that by default ElementBView is derived from ElementBViewBase.

If we want the View to be derived from ElementAView, then we have to explicitly say by right-clicking the View and selecting the base View. I’ll set it to ElementAView now. 

In the code, we can see that now ElementBViewViewBase,which is the parent class for ElementBView, is derived from ElementAView.

Inheritance is also supported for the ViewComponents. If you have a ViewComponent which is going to be a base, we can connect it to the View, but you can also create other components which will be derived from the base component. For this, you create another component and simply connect those just like you connected Elements. Now ConcreteComponent will be derived from the BaseComponent. 

If you’re not going to instantiate ElementA, but only use it as the base for ElementB and other Elements, you can right-click it and set it as Abstract. Now you won’t be able to instantiate it. However, you will still be able to use it as a base class.