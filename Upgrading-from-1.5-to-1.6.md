#1.5 to 1.6 Upgrade

##Step 1:
Create a new uFrameProject, in the same folder as your old project file, right-click on the folder and select uFrame->New Project.

##Step 2:
Now you need to convert your graph files, select each graph file and choose "Upgrade Format" in the inspector.  This will create a new file 
called "{OldName}-New".

##Step 3:
Click on your project to view it in the inspector. Now add the newly created diagram file to the project by selection the '+' button next to 
project diagrams.

##Step 4:
At this point you should be able to select your project in the uFrame designer.  If there are any project issues ("Noted in the project inspector") you will need to correct them as specified.

##Step 5:
IMPORTANT: Create a new folder with the name of your graph and throw all uFrame 1.5 code into this folder.

##Step 6:
Delete all "*.designer.cs" files from this folder.

##Step 7:
Now Save & Compile your project.

#Code Updates
## ViewModel base classes
All Editable ViewModel files need to explicitly derive from its base.  1.5 Editable ViewModels look like this

###OLD
`public partial class MyViewModel {`

###NEW

`public partial class MyViewModel : MyViewModelBase {`

## View base classes
All Editable View files need to explicitly derive from its base.  1.5 Editable View look like this

###OLD 1.5
`public partial class MyView {`

###NEW 1.6

`public partial class MyView : MyViewBase {`

## Scene Manager Settings base class
###OLD 1.5
    using System;
    using System.Collections;
    using System.Collections.Generic;
    using System.Linq;

    public sealed partial class MySceneManagerSettings {

###NEW 1.6
    using System;
    using System.Collections;
    using System.Collections.Generic;
    using System.Linq;

    [System.SerializableAttribute()]
    public class MySceneManagerSettings : MySceneManagerSettingsBase {

## Collection View Bindings
You'll need to fix the overrides for this binding type there has been a name format change.
Old: Create{CollectionName}View
  /// This binding will add or remove views based on an element/viewmodel collection.
    public override ViewBase CreateAsteroidsView(AsteroidViewModel item)
    {
        ..etc
    }
New: {CollectionName}CreateView

    public override ViewBase AsteroidsCreateView(ViewModel arg1)
    {
           ..etc
    }

  
This change has occurred because of a more generic binding system.  Its an annoying change, but the benefits of the new binding system will make up for it.




