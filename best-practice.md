# 1. Project guidelines

## 1.2 Package structure 

We use a feature-based project structure. e.g.
#### App
* ui
    * login
    * signup
    * repo
    * userlist

* data
    * account
    * repo
    * user

## 1.3 File naming 

### 1.3.1 Class files
Class names are written in [UpperCamelCase](http://en.wikipedia.org/wiki/CamelCase). 

For classes that extend an Android component, the name of the class should end with the name of the component; for example: `SignInActivity`, `SignInFragment`, `ImageUploaderService`, `ChangePasswordDialog`.

### 1.3.1 Resources files

Resources file names are written in __lowercase_underscore__. 

#### 1.3.1.1 Drawable files

ANGUS: I'm up to changing this... not too keen on it.
 
Naming conventions for drawables:


| Asset Type   | Prefix            |        Example               |
|--------------| ------------------|-----------------------------|
| Action bar   | `ab_`             | `ab_stacked.9.png`          |
| Button       | `btn_`             | `btn_send_pressed.9.png`    |
| Dialog       | `dialog_`         | `dialog_top.9.png`          | 
| Divider      | `divider_`        | `divider_horizontal.9.png`  |
| Icon         | `ic_`              | `ic_star.png`               |
| Menu         | `menu_ `           | `menu_submenu_bg.9.png`     |
| Notification | `notification_`    | `notification_bg.9.png`     |
| Tabs         | `tab_`            | `tab_pressed.9.png`         |

Naming conventions for icons (taken from [Android iconography guidelines](http://developer.android.com/design/style/iconography.html)):

| Asset Type                      | Prefix             | Example                      |
| --------------------------------| ----------------   | ---------------------------- | 
| Icons                           | `ic_`              | `ic_star.png`                |
| Launcher icons                  | `ic_launcher`      | `ic_launcher_calendar.png`   |
| Menu icons and Action Bar icons | `ic_menu`          | `ic_menu_archive.png`        |
| Status bar icons                | `ic_stat_notify`   | `ic_stat_notify_msg.png`     |
| Tab icons                       | `ic_tab`           | `ic_tab_recent.png`          |
| Dialog icons                    | `ic_dialog`        | `ic_dialog_info.png`         |

Naming conventions for selector states:

| State        | Suffix          | Example                     |
|--------------|-----------------|-----------------------------|
| Normal       | `_normal`       | `btn_order_normal.png`      |
| Pressed      | `_pressed`      | `btn_order_pressed.png`     |
| Focused      | `_focused`      | `btn_order_focused.png`     |
| Disabled     | `_disabled`     | `btn_order_disabled.png`    |
| Selected     | `_selected`     | `btn_order_selected.png`    |
| Selector     | `_selector`     | `btn_order_selector.png`    |


#### 1.3.1.2 Layout files

Layout files should match the name of the Android components that they are intended for but moving the top level component name to the end. For example, if we are creating a layout for the `SignInActivity`, the name of the layout file should be `sign_in_activity.xml`. This is so that we keep all related layouts together (feature-based).

| Component        | Class Name             | Layout Name                   |
| ---------------- | ---------------------- | ----------------------------- |
| Activity         | `UserProfileActivity`  | `user_profile_activity.xml`   |
| Fragment         | `SignUpFragment`       | `sign_up_fragment.xml`        |
| Dialog           | `ChangePasswordDialog` | `change_password_dialog.xml`  |
| AdapterView item | ---                    | `person_item.xml`             |
| View             | TemperatureView        | `stats_bar_view.xml`          |

A slighly different case is when we are creating a layout that is going to be inflated by an `Adapter`, e.g to populate a `ListView`. In this case, the name of the layout should end with `_item`

Note that there are cases where these rules will not be possible to apply. For example, when creating layout files that are intended to be part of other layouts. In this case you should use the postfix `_view`.

#### 1.3.1.3 Menu files  

Similar to layout files, menu files should match the name of the component. For example, if we are defining a menu file that is going to be use in the `UserActivity`, then the name of the file should be `user_activity.xml`

A good practise is to not include the word `menu` as part of the name because these files are already located in directory called menu.

#### 1.3.1.4 Values files

Resource files in the values folder should be __plural__, e.g. `strings.xml`, `styles.xml`, `colors.xml`, `dimens.xml`, `attrs.xml`

#### 1.3.1.5 Strings

String resource files should be postfixed with the feature they relate to. e.g. `strings-core.xml`, `strings-login.xml`, `strings-signup.xml`.

This is so that we don't end up with a monolithic `strings.xml` in large projects.

# 2 Code guidelines 

## 2.1 Java language rules

### 2.1.1 Don't ignore exceptions

You must never do the following:

```java
void setServerPort(String value) {
    try {
        serverPort = Integer.parseInt(value);
    } catch (NumberFormatException e) { }
}
```

_While you may think that your code will never encounter this error condition or that it is not important to handle it, ignoring exceptions like above creates mines in your code for someone else to trip over some day. You must handle every Exception in your code in some principled way. The specific handling varies depending on the case._ - ([Android code style guidelines](https://source.android.com/source/code-style.html))

See alternatives [here](https://source.android.com/source/code-style.html#dont-ignore-exceptions).
    
### 2.1.2 Don't catch generic exception

You should not do this:

```java
try {
    someComplicatedIOFunction();        // may throw IOException 
    someComplicatedParsingFunction();   // may throw ParsingException 
    someComplicatedSecurityFunction();  // may throw SecurityException 
    // phew, made it all the way 
} catch (Exception e) {                 // I'll just catch all exceptions 
    handleError();                      // with one generic handler!
}
```

See the reason why and some alternatives [here](https://source.android.com/source/code-style.html#dont-catch-generic-exception)

### 2.1.3 Don't use finalizers

_We don't use finalizers. There are no guarantees as to when a finalizer will be called, or even that it will be called at all. In most cases, you can do what you need from a finalizer with good exception handling. If you absolutely need it, define a `close()` method (or the like) and document exactly when that method needs to be called. See `InputStream` for an example. In this case it is appropriate but not required to print a short log message from the finalizer, as long as it is not expected to flood the logs._ - ([Android code style guidelines](https://source.android.com/source/code-style.html#dont-use-finalizers))


### 2.1.4 Fully qualify imports

This is bad: `import foo.*;`

This is good: `import foo.Bar;`

See more info [here](https://source.android.com/source/code-style.html#fully-qualify-imports)

## 2.2 Java style rules 

### 2.2.1 Fields definition and naming 

Fields should be defined at the __top of the file__ and they should follow the naming rules listed below.

* All fields start with a lower case letter.
* Static final fields (constants) are ALL_CAPS_WITH_UNDERSCORES.

Example:

```java
public class MyClass {
    public static final int SOME_CONSTANT = 42;
    public int publicField;
    private static MyClass singleton;
    int packagePrivate;
    private int private;
    protected int protected;
}
```
    
### 2.2.3 Treat acronyms as words

| Good           | Bad            |
| -------------- | -------------- |
| `XmlHttpRequest` | `XMLHTTPRequest` | 
| `getCustomerId`  | `getCustomerID`  | 
| `String url`     | `String URL`     |
| `long id`        | `long ID`        |


### 2.2.7 Limit variable scope 

_The scope of local variables should be kept to a minimum (Effective Java Item 29). By doing so, you increase the readability and maintainability of your code and reduce the likelihood of error. Each variable should be declared in the innermost block that encloses all uses of the variable._ 

_Local variables should be declared at the point they are first used. Nearly every local variable declaration should contain an initializer. If you don't yet have enough information to initialize a variable sensibly, you should postpone the declaration until you do._ - ([Android code style guidelines](https://source.android.com/source/code-style.html#limit-variable-scope))

### 2.2.9 Logging guidelines

Use Timber for logging.
    
### 2.2.10 Class member ordering 

There is no single correct solution for this but using a __logical__ and __consistent__ order will improve code learnability and readability. It is recommendable to use the following order:

1. Constants 
2. Fields 
3. Constructors 
4. Override methods and callbacks (public or private)
5. Public methods
6. Private methods
7. Inner classes or interfaces

Example:

```java
public class MainActivity extends Activity {

    private String mTitle;
    private TextView mTextViewTitle;
    
    public void setTitle(String title) {
        mTitle = title;
    }
    
    @Override 
    public void onCreate() {
        ...
    }
    
    private void setUpView() {
        ...
    }
    
    static class AnInnerClass {
    
    }

} 
```

If your class is extending and __Android component__ such as an Activity or a Fragment, it is a good practise to order the override methods so that they __match the component's lifecycle__. For example, if you have an Activity that implements `onCreate()`, `onDestroy()`, `onPause()` and `onResume()`, then the correct order is:

```java
public class MainActivity extends Activity {

    //Order matches Activity lifecycle  
    @Override 
    public void onCreate() {} 
    
    @Override 
    public void onResume() {}
    
    @Override 
    public void onPause() {}
    
    @Override 
    public void onDestory() {}

}
```

### 2.2.11 Parameter ordering in methods

When programming for Android, it is quite common to define methods that take a `Context`. If you are writing a method like this, then the __Context__ must be the __first__ parameter.

The opposite case are __callback__ interfaces that should always be the __last__ parameter.

Examples:

```java
// Context always go first
public User loadUser(Context context, int userId);

// Callbacks always go last
public void loadUserAsync(Context context, int userId, UserCallback callback);
```

### 2.2.13 String constants, naming and values

Many elements of the Android SDK such as `SharedPreferences`, `Bundle` or `Intent` use a key-value pair approach so it's very likely that even for a small app you end up having to write a lot of String constants.

When using one of these components, you __must__ define the keys as a `static final` fields and they should be prefixed as indicaded below. 

| Element            | Field Name Prefix |
| -----------------  | ----------------- |
| SharedPreferences  | `PREF_`             |
| Bundle             | `BUNDLE_`           | 
| Fragment Arguments | `ARGUMENT_`         |   
| Intent Extra       | `EXTRA_`            |
| Intent Action      | `ACTION_`           |

Note that the arguments of a Fragment - `Fragment.getArguments()` - are also a Bundle. However, because this is a quite common use of Bundles, we define a different prefix for them. 

Example:

```java
// Note the value of the field is the same as the name to avoid duplication issues
static final String PREF_EMAIL = "PREF_EMAIL";
static final String BUNDLE_AGE = "BUNDLE_AGE";
static final String ARGUMENT_USER_ID = "ARGUMENT_USER_ID";

// Intent-related items use full package name as value
static final String EXTRA_SURNAME = "com.myapp.extras.EXTRA_SURNAME";
static final String ACTION_OPEN_USER = "com.myapp.action.ACTION_OPEN_USER";
```

### 2.2.14 Arguments in Fragments and Activities

When data is passed into an `Activity `or `Fragment` via `Intents` or a `Bundles`, the keys for the different values __must__ follow the rules described in the section above.

When an `Activity` or `Fragment` expect arguments, it should provide a `static public` factory method that facilitates the creation of the `Fragment` or `Intent`.

In the case of Activities the method is usually called `intent()`

```java
public static Intent intent(Context context, User user) {
    Intent intent = new Intent(context, ThisActivity.class);
    intent.putParcelableExtra(EXTRA_USER, user);
    return intent;
}
```

For Fragments it's named `newInstance()` and it handles the creation of the Fragment with the right arguments. 

```java
public static UserFragment newInstance(User user) {
    UserFragment fragment = new UserFragment;
    Bundle args = new Bundle();
    args.putParcelable(ARGUMENT_USER, user);
    fragment.setArguments(args)
    return fragment;
}
```

__Note 1__: these methods should go at the top of the class before `onCreate()`

__Note 2__: if we provide the methods described above, the keys for extras and arguments should be `private` because there is not need for them to be exposed outside the class. 

### 2.3.2 Resources naming 

Resource IDs and names are written in __lowercase_underscore__

#### 2.3.2.1 ID naming

IDs should be postfixed with the name of the element in lowercase underscore. For example:


| Element            | Postfix            |
| -----------------  | ----------------- |
| `TextView`           | `_text`             |
| `ImageView`          | `_image`            | 
| `Button`             | `_button`           |   
| `Menu`               | `_menu`             |

Image view example:

```xml
<ImageView
    android:id="@+id/profile_image"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" 
    />
```

Menu example:

```xml
<menu>
    <item
        android:id="@+id/done_menu"
        android:title="Done" />
</menu>
```

#### 2.3.2.2 Strings

Should be namespaced like: `login.button.text`, `login.email.hint`, `login.password.hint`.

This gives translators context aswell as allows other developers to autocomplete easily.

#### 2.3.2.3 Styles and Themes

Unlike the rest of resources, style names are written in __UpperCamelCase__.

 * Themes and Styles are different. Themes should be in `themes.xml` styles should be in `styles.xml`.
 * Themes apply to the entire app where as styles apply to an element.

See: http://blog.danlew.net/2014/11/19/styles-on-android/