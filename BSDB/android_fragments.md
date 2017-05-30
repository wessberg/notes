# Android Fragments

> [Fragments (guide)](https://developer.android.com/guide/components/fragments.html)
>
> [Fragments (tutorial)](https://developer.android.com/training/basics/fragments/index.html)
>
> [Supporting Tablets and Handsets](https://developer.android.com/guide/practices/tablets-and-handsets.html)

## Fragments

A `Fragment` represents a behavior or a portion of a user interface in an `Activity`.

A Fragment is a modular section of an activity which has its own lifecycle, receives its own input events and which you can add or remove while the activity is running.

When the parent Activity is paused or destroyed, so are all fragments inside of it.

### Philosophy

By dividing the layout of an activity into fragments, you become able to modify the activity's appearance at runtime and preserve those changes in a back stack that's managed by the activity.

For example, you could have a fragment for a left and right sidebar, each with its own set of lifecycle callback methods and each responsible for handling their own user input events.

### Modularity in Fragments

Each fragment defines its own layout, lifecycle callbacks and behavior, so you can include one fragment in multiple activities. Thus, its best to design for reuse.

### Creating a Fragment

To create a fragment, you must create a subclass of `Fragment`.

### Fragment lifecycles

The 3 most important ones are:

#### `onCreate()`

Called when the fragment is created. Here, you should initialize essential components of the fragment that you want to retain when the fragment is paused or stopped, then resumed.

#### `onCreateView()`

Called when it's time for the fragment to draw its user interface for the first time. **To draw a UI for your fragment, you must return a `View` from this method that is the root of your fragment's layout. You can return null if the fragment does not provide a UI**.

#### `onPause()`

Called when the user is leaving the fragment.

### Adding a user interface to a Fragment

To add some UI to a Fragment, you must implement the `onCreateView()` callback method, which the Android system calls when it's time for the fragment to draw its' layout. You must return a `View` that is the root of your fragment's layout!

You can inflate it from a layout resource defined in XML. A `LayoutInflater` is passed to the method as the first argument. So, one imagined way to implement the method would be:

```java
@Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        // Inflate the layout for this fragment
        return inflater.inflate(R.layout.example_fragment, container, false);
    }
```

### Adding a Fragment to an Activity

There are two ways to do it:

#### Declare the fragment inside the activity's layout file

You can just use the fragment as if it were a view inside the activity's layout file:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <fragment android:name="com.example.news.ArticleListFragment"
            android:id="@+id/list"
            android:layout_weight="1"
            android:layout_width="0dp"
            android:layout_height="match_parent" />
    <fragment android:name="com.example.news.ArticleReaderFragment"
            android:id="@+id/viewer"
            android:layout_weight="2"
            android:layout_width="0dp"
            android:layout_height="match_parent" />
</LinearLayout>
```

Notice how the `name` attributeselects the proper `Fragment` class to instantiate in the layout.

**It is important that you give your fragment either an ID or a tag so that Android knows how to restore the fragment if the activity is restarted!**

#### Add the fragment programatically to an existing `ViewGroup`

You can always just add a fragment to a `ViewGroup` imperatively:

```java
FragmentManager fragmentManager = getFragmentManager();
FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();

ExampleFragment fragment = new ExampleFragment();
fragmentTransaction.add(R.id.fragment_container, fragment);
fragmentTransaction.commit();
```

Where the first argument given to `add()` is the `ViewGroup` in which the fragment should be placed, specified by resource ID.

### The `FragmentManager`

The `FragmentManager` can find all fragments in a given `Activity` as well as pop fragments off the back stack and register listeners for changes to the back stack.

To get a `FragmentManager`, call `getFragmentManager()` from your `Activity`.

### Accessing the parent Activity from a Fragment

A Fragment can get a reference to the Activity that is currently holding it with the `getActivity()` method.