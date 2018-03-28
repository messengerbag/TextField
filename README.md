# TextField
TextField is a script module for [Adventure Game Studio (AGS)](http://www.adventuregamestudio.co.uk/). It requires AGS v3.4.0 or higher.

This module provides one-line text input fields, to replace the built-in AGS `TextInput` control. Its main benefit is that it allows you to position the text cursor (caret) freely within the input string, using either arrow keys or mouse. It also has a notion of focus, which makes it easier to have GUIs with multiple text fields. Finally, you can customize the appearance and behavior of the text fields somewhat.

## How To Use
To use, you have to set up a GUI Button for each text field you want. The position, size, font, and text color of the button will be used to format the text field. Then you have to initialize the text field using `TextField.Create()`, providing the GUI button as an argument, like so:

```adventure-game-studio
TextField* myTextField = TextField.Create(myButton);
```

You would typically do this in `game_start()`, and in any case before the text field is displayed. **After you've created the text field, don't set the button properties directly!**

You also need to hook up the events. Because we lose the TextInput OnActivate event, we have to handle activating the field (typically by pressing Return) a little differently. There are two alternatives: You can handle it yourself in the game's general `on_key_press()` function:

```adventure-game-studio
function on_key_press(eKeyCode keycode)
{
  if(keycode == eKeyReturn)
  {
    if(TextField.Focused == myTextField)
    {
      // Activate
    }
    else if(TextField.Focused == myOtherTextField)
    {
      // Activate
    }
    // ...
  }
  else // handle other keys
}
```
Or the module can handle it, by setting `TextField.HandleReturn = true;` but then you have to check `textField.Activated()` each game cycle to see if it was activated:

```adventure-game-studio
function repeatedly_execute_always()
{
  if(myTextField.Activated())
  {
    // Activate
  }
  else if(myOtherTextField.Activated())
  {
    // Activate
  }
  /...
}
```

You should also link the button's `OnClick()` event to a function to handle clicks in the text field. Default behavior (set focus and position the text cursor) is provided, so you can just do:

```adventure-game-studio
function myButton_OnClick(GUIControl *control, MouseButton button)
{
  myTextField.HandleMouseClick(button);
}
```

Or, if you want a common function you can use for all the text fields:

```adventure-game-studio
function myTextFieldButtons_OnClick(GUIControl *control, MouseButton button)
{
  TextField.HandleMouseClickAny(control, button);
}
```

## Licensing
This code is offered under multiple licenses. Choose whichever one you like.

You may use it under the MIT license:
- https://opensource.org/licenses/MIT

You may also use it under the Creative Commons Attribution 4.0 International License:
- https://creativecommons.org/licenses/by/4.0/

You may also use it under the Artistic License 2.0:
- https://opensource.org/licenses/Artistic-2.0
