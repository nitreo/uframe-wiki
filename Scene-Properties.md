# Scene Properties

Sometimes there's a need to update [_ViewModel's_](ViewModel) property directly from a [_View_](Views). In such case you can create a _Scene Property_.

In order to create a _Scene Property_, create a normal property on an [_Element_](Elements) and then connect it to the [_View_](Views).

[picture]

After recompilation, uFrame will add to the View new methods that allow to update the _Scene Property_.

## Update methods

`Calculate{PropertyName}`

`Get{PropertyName}Observable`

This method returns an observable so you can use it to control when/how often the _Scene Property_ gets updated and with what values.

[example]

If this method is implemented then the `Calculate{PropertyName}` won't be called.

## Resources
[uFrame 1.5 - Scene Properties Youtube Video](https://www.youtube.com/watch?v=shlvL6Fhq8o)