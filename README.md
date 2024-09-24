# Office 97 style Colour Picker control

A simple drop in color chooser control

![Colour Picker image](https://raw.githubusercontent.com/ChrisMaunder/colour_picker/master/docs/assets/colour_picker.gif)

In an effort to have the latest and greatest wizz-bang features in
my programs I unashamedly ripped of the colour picker from Office 97.

Initially I tried to modify an owner drawn combo box and combine
that with a multicolumn combobox, but current multicolumn combo
boxes are really just a single column with dividing lines drawn in.
I then decided to write the whole thing from scratch based on a button,
since it would at least give me a `BN_CLICKED` notification to get
things started.

The colour picker is in two parts: an owner drawn button that
reflects the currently selected colour, and a popup colour chooser
window to select this colour. When the user clicks on the button
the popup window appears and all mouse messages are captured until
the left mouse button is clicked, or until the Enter or Escape keys
are pressed. The popup window can be navigated using the mouse or
the keyboard and includes tooltips explaining what each colour is.

The control can be incorporated into a project like any other
`CButton` derived control. Either Create the control manually, subclass
an existing CButton or `DDX_control` it. The control also comes with
a `DDX_ColourPicker` routine to get/set the colour of 
the control using a variable of type `COLORREF`.

The Colour Picker is contained in the class `CColourPicker`.
It uses the class `CColourPopup` for the popup window.
These classes are contained in the file [colour_picker_src.zip](https://raw.githubusercontent.com/ChrisMaunder/colour_picker/master/docs/assets/colour_picker_src.zip), and a sample project is contained in
[colour_picker_demo.zip](https://raw.githubusercontent.com/ChrisMaunder/colour_picker/master/docs/assets/colour_picker_demo.zip). 

CColourPicker only has the following public member functions:

```cpp
void     SetColour(COLORREF crColour);
COLORREF GetColour();

void     SetDefaultText(LPCTSTR szDefaultText);
void     SetCustomText(LPCTSTR szCustomText);

void     SetSelectionMode(UINT nMode); // Either CP_MODE_TEXT or CP_MODE_BK

UINT     GetSelectionMode();

void     SetBkColour(COLORREF crColourBk);
COLORREF GetBkColour();
  
void     SetTextColour(COLORREF crColourText);
COLORREF GetTextColour();
```

`SetDefaultText` allows you to set the text that will appear in the "Default"
area of the colour chooser. If you pass NULL, then the Default area will not be available
to the user. If this area is availble and the user selects it, the value `CLR_DEFAULT`
will be returned.

`SetCustomText` allows you to set the text that will appear in the "Custom"
area of the colour chooser. If you pass NULL, then the Custom area will not be available
to the user. The Custom area allows the user to select a custom colour using the
standard windows colour selection dialog.

You can choose whether the colour chosen using the dropdown colour chooser will
affect the text or the background colour using the function
`SetSelectionMode(int nMode)`. Possible values for nMode are `CP_MODE_TEXT`
to make colour changes affect the text colour, and `CP_MODE_BK` to make changes
affect the background (default). 

`SetColour`, `GetColour` and the the DDX-function
will set and get the colour according to the current selection mode. To access
the text colour and the background colour directly use the `Set/GetTextColour`
and `Set/GetBkColour` functions.

There are also a number of user messages that may be handled to
get more information from the control. These are:

<center>
  <table border="" width="90%" bgcolor="#FFFFCC">
    <tr>
      <th align="LEFT" width="20%">Message</th>
      <th align="LEFT">Description</th>
    </tr>
    <tr>
      <td width="30%">CPN_SELCHANGE</td>
      <td>Colour Picker Selection change</td>
    </tr>
    <tr>
      <td width="30%">CPN_DROPDOWN</td>
      <td>Colour Picker drop down</td>
    </tr>
    <tr>
      <td width="30%">CPN_CLOSEUP</td>
      <td>Colour Picker close up</td>
    </tr>
    <tr>
      <td width="30%">CPN_SELENDOK</td>
      <td>Colour Picker end OK</td>
    </tr>
    <tr>
      <td width="30%">CPN_SELENDCANCEL</td>
      <td>Colour Picker end (cancelled)</td>
    </tr>
  </table>
</center>

These messages can be handled using `ON_MESSAGE(< MESSAGE>, MessageFn)
` in you message map entries, where `MessageFn` is of
the form

```cpp
afx_msg LONG MessageFn(UINT lParam, LONG wParam);
```

The demo program gives an example of how to do this.

## Acknowledgments

Alexander Bischofberger kindly
supplied the Selection mode modifications, as well as the background and text color methods.
Paul Wilkerson fixed a focus related bug, and Geir Arne Trillhus also helped fix a few bugs.

