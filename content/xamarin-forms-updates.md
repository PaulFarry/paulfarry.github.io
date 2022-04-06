# Updates to Xamarin.Forms

When you update the Xamarin.Forms libraries in a project ensure that the first step afterwards is to Completely Clean the solution and rebuild.

There are a number of errors that are experienced if you do not, and these are obsure.

Predominately  `InvalidCastException` between obviously incompatible types.

```
System.InvalidCastException: Unable to convert instance of type 'Android.Support.Design.Widget.TabLayout' 
to type 'Android.Support.V7.Widget.Toolbar'.
```
