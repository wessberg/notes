# Android Menus & Navigation

> [Menus](https://developer.android.com/guide/topics/ui/menus.html)

## Menus

Android provides several `Menu` APIs to present user actions and options in your activities.

### Fundamental types of menus or action presentations

There are 3:

#### Options menu and app bar

This is the primary collection of menu items for an activity.

This is where you place actions that have a global impact on the app, such as "Search", "Compose email" and "Settings".

#### Context menu and contextual action mode

A context menu is a floating menu that **appears when the user performs a long-click on an element**.

It provides actions that affect the selected content or context frame.

#### Popup menu

A list of items in a vertical list that's anchored to the view that invoked the menu.

### Defining a Menu in XML

Android suggests that you define a menu and all its items in an XML menu **resource**, rather than inside an activity.

You simply create an XML file inside your projects `res/menu/` directory and build it with the following elements:

- `<menu>`: Defines a Menu, which is a container for menu items.
- `<item>`: Creates a `MenuItem`, which represents a single item in a menu. May contain a nested <menu> element.
- `<group>`: An optional, invisible container for <item> elements.

#### Example

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/new_game"
          android:icon="@drawable/ic_new_game"
          android:title="@string/new_game"
          android:showAsAction="ifRoom"/>
    <item android:id="@+id/help"
          android:icon="@drawable/ic_help"
          android:title="@string/help" />
</menu>
```

Notice the `showAsAction` boolean attribute. It decides if the item should appear as an action item or not.

## Handling click events

Override the inherited instance method `onOptionsItemSelected()`:

```java
@Override
public boolean onOptionsItemSelected(MenuItem item) {
    // Handle item selection
    switch (item.getItemId()) {
        case R.id.new_game:
            newGame();
            return true;
        case R.id.help:
            showHelp();
            return true;
        default:
            return super.onOptionsItemSelected(item);
    }
}
```

If you don't end up switching views, return false (the overriden method does that by default, so just returning the super call satisfies this constraint).

## Contextual menus

A contextual menu offers actions that affect a specific item or context frame in the UI.

There a multiple kinds of contextual menus:

### Floating context menus

Floating contet menus appears as floating lists of menu items (similar to a dialog) when the user performs a long-click (press and hold) on a view that declares support for a context menu.

### Contextual action mode

This mode displays a *contextual action bar* at the top of the screen with action items that affect the selected item(s). When this mode is active, users can perform an action on multiple items at once if your app allows it.

## Creating a floating context menu

1. Register the View to which the context menu should be associated by calling `registerForContextMenu()` and pass it the View.
2. Implement the `onCreateContextMenu()` method in your `Activity` or `Fragment`.
3. Implement `onContextItemSelected()` and take action based on whatever was clicked by the user.

## Creating a Popup Menu

This is useful for:

- Providing an overflow-style menu for actions that *relate* to specific content.
- Providing a second part of a command sentence.
- Providing a drop-down that does not retain a persistent selection.

Here's how to create it:

1. Instantiate a `PopupMenu` with its constructor, which takes the current application `Context` and the `View` to which the menu should be anchored.
2. Use `MenuInflator` to inflate your menu resource into the `Menu` object returned by `PopupMenu.getMenu()`.
3. Call `PopupMenu.show()`

### Example of creating a Popup Menu

```xml
<ImageButton
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:src="@drawable/ic_overflow_holo_dark"
    android:contentDescription="@string/descr_overflow_button"
    android:onClick="showPopup" />
```

```java
public void showPopup(View v) {
    PopupMenu popup = new PopupMenu(this, v);
    MenuInflater inflater = popup.getMenuInflater();
    inflater.inflate(R.menu.actions, popup.getMenu());
    popup.show();
}
```