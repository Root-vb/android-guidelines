# Android Project Guidelines
---------------------------


## 1. Project Guidelines

### 1.1 Project Structure

New projects should follow the Android Gradle project structure that is defined on the [Android Gradle plugin user guide](http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-Project-Structure).


### 1.2 File Naming

#### 1.2.1 Class Files

Classes should be named using [UpperCamelCase](http://en.wikipedia.org/wiki/CamelCase).

Classes extending an Android framework component should **always** end with the component name.

Example:

    LoginFragment, SignUpActivity, VerificationDialog, MyFirebaseMessagingService


#### 1.2.1 Resource Files

Resource files should be named using lowercase letters and underscores instead of spaces: **lowercase_underscore**.

Example:

    activity_main, fragment_user, item_post


#### 1.2.2.1 Drawable Files

P.S: **Always use vector drawables**, if possible.

Use [Asset Studio](https://romannurik.github.io/AndroidAssetStudio/) to generate assets such as launcher icons for your Android app.

For **SVG'S** use [FlatIcon](http://www.flaticon.com/), [The noun project](https://thenounproject.com/) and then import them using Android Studio to Vector Drawables

For generating **Animated Vector Drawables** use this awesome product [Shape Shifter](https://shapeshifter.design/)

Drawable resource files should be named using the **ic_** prefix along with the size and color of the asset. For example, white accept icon sized at 24dp would be named:

	ic_accept_24dp_white

And a black cancel icon sized at 48dp would be named:

	ic_cancel_48dp_black

Other drawable files should be named using the corresponding prefix, for example:

| Type       | Prefix    | Example                |
|------------|-----------|------------------------|
| Selector   | selector_ | selector_button_cancel |
| Background | bg_       | bg_rounded_button      |
| Progress   | progress_ | progress_purple        |
| Animated Vector Drawable | avd_ |avd_logo       |


#### 1.2.2.2 Layout Files

When naming layout files, they should be named starting with the name of the Android Component that they have been created for. For example:

| Component        | Class Name      | Layout Name       |
|------------------|-----------------|-------------------|
| Activity         | MainActivity    | activity_main     |
| Fragment         | MainFragment    | fragment_main     |
| Dialog           | RateDialog      | dialog_rate       |
| AdapterView Item | N/A             | item_follower     |
| Partial layout   | N/A             | layout_toolbar    |

Not only does this approach makes it easy to find files in the directory hierarchy, but it really helps when needing to identify what corresponding class a layout file belongs to.


#### 1.2.2.3 Menu Files

Menu files do not need to be prefixed with the menu_ prefix. This is because they are already in the menu package in the resources directory, so it is not a requirement.

#### 1.2.2.4 Values Files

All resource file names should be **plural**, for example:

	attrs.xml, strings.xml, styles.xml, colors.xml, dimens.xml



## 2. Code Guidelines

### 2.1 Java Language Rules

#### 2.1.1 Never ignore exceptions

Avoid not handling exceptions like this. 

Example:

```java
void setServerPort(String value) {
    try {
        serverPort = Integer.parseInt(value);
    } catch (NumberFormatException e) { }
}
```

_While you may think that your code will never encounter this error condition or that it is not important to handle it, ignoring exceptions like above creates mines in your code for someone else to trip over some day. You must handle every Exception in your code in some principled way. The specific handling varies depending on the case._ - ([Android code style guidelines](https://source.android.com/source/code-style.html))

See best practices to handle exception [here](https://source.android.com/source/code-style.html#dont-ignore-exceptions).


#### 2.1.2 Never catch generic exceptions


You should avoid doing this:

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


#### 2.1.3 Using try-catch over throw exception

Using try-catch statements improves the readability of the code where the exception is taking place. This is because the error is handled where it occurs, making it easier to both debug or make a change to how the error is handled.


#### 2.1.4 Never use Finalizers

_We don't use finalizers. There are no guarantees as to when a finalizer will be called, or even that it will be called at all. In most cases, you can do what you need from a finalizer with good exception handling. If you absolutely need it, define a `close()` method (or the like) and document exactly when that method needs to be called. See `InputStream` for an example. In this case it is appropriate but not required to print a short log message from the finalizer, as long as it is not expected to flood the logs._ - ([Android code style guidelines](https://source.android.com/source/code-style.html#dont-use-finalizers))


#### 2.1.5 Fully qualify imports

When declaring imports, use the full package declaration. For example:

Don’t do this:


    import android.support.v7.widget.*;

Instead, do this


    import android.support.v7.widget.RecyclerView;


#### 2.1.6 Don't keep unused imports

Sometimes removing code from a class can mean that some imports are no longer needed. If this is the case then the corresponding imports should be removed alongside the code.


### 2.2 Java Style Rules

#### 2.2.1 Field definition and naming

Fields should be defined at the **top of the file** and they should follow the naming rules listed below.

* Private, non-static field names start with **m**.
* Private, static field names start with **s**.
* Other fields start with a lower case letter.
* Static final fields (constants) are ALL_CAPS_WITH_UNDERSCORES.

Example:

```java
public class MyClass {
    public static final int SOME_CONSTANT = 42;
    public int publicField;
    private static MyClass sSingleton;
    int mPackagePrivate;
    private int mPrivate;
    protected int mProtected;
}
```

Field names that do not reveal intention should not be used. For example,

    int e; //number of elements in the list

why not just give the field a meaningful name in the first place, rather than leaving a comment!

    int numberOfElements;

That's much better!


#### 2.2.1.2 View Field Naming

When naming fields that reference views, the name of the view should be the last word in the name. 

Example:

| View           | Name              |
|----------------|-------------------|
| TextView       | usernameTextView      |
| Button         | acceptLoginButton   |
| ImageView      | profileAvatarImageView |
| RelativeLayout | profileView    |


#### 2.2.2 Avoid naming with container types

Avoid the use of container type names when creating variables for collections. For example, say we have an arraylist containing a list of userIds:

Do:

    List<String> userIds = new ArrayList<>();

Don't:

    List<String> userIdList = new ArrayList<>();


#### 2.2.3 Number series naming

Android Studio auto-generates code for us, it's easy to leave things as they are - even when it generate horribly named parameters! For example, this isn't very nice:

	public void doSomething(String s1, String s2, String s3)

It's hard to understand what these parameters do without reading the code. Instead:

	public void doSomething(String userName, String userEmail, String userId)

That makes it much easier to understand! Now we'll be able to read the code following the parameter with a much clearer understanding.


#### 2.2.4 Treat acronyms as words

Any acronyms for class names, variable names etc should be treated as words - this applies for any capitalisation used for any of the letters. 

Example:

| Good            | Bad             |
|-----------------|-----------------|
| setApiToken     | setAPIToken     |
| String uri      | String URI      |
| int id          | int ID          |
| parseHtml       | parseHTML       |
| generateJsonFile| generateJSONFile|


#### 2.2.5 Avoid justifying variable declarations

Any declaration of variables should not use any special form of alignment, for example:

This is fine:

    private int mUserId = 8;
    private int mCount = 0;
    private String mUserName = "squareboat";

Avoid doing this:

    private String mUserName = "squareboat";
    private int mUserId      = 8;
    private int mCount       = 0;
    

#### 2.2.6 Use spaces for indentation


For blocks, **4 space** indentation should be used:


    if (mUserSignedIn) {
        count = 1;
    }

Whereas for line wraps, **8 spaces** should be used:


    String mUserAboutText =
            "This is some text about the user and it is pretty long, can you see!"


### 2.2.7 If-Statements

#### 2.2.7.1 Use standard brace style

Braces should always be used on the same line as the code before them. For example, avoid doing this:


    class SomeClass
    {
    	private void someFunction()
    	{
        	if (isSomething)
        	{

        	}
        	else if (!isSomethingElse)
        	{

        	}
        	else
        	{

        	}
    	}
	}

And instead, do this:


	class SomeClass {
    	private void someFunction() {
        	if (isSomething) {

        	} else if (!isSomethingElse) {

        	} else {

        	}
    	}
	}

Not only is the extra line for the space not really necessary, but it makes blocks easier to follow when reading the code.

#### 2.2.7.2 Inline if-clauses

Sometimes it makes sense to use a single line for if statements. For example:

    if (mUser == null) return false;

However, it only works for simple operations. Something like this would be better suited with braces:


    if (mUser == null) throw new IllegalArgumentExeption("Oops, user object is required.");

#### 2.2.7.3 Nested if-conditions

Where possible, if-conditions should be combined to avoid over-complicated nesting. For example:

Do:


    if (mUserSignedIn && mUserId != null) {

    }

Try to avoid:


    if (mUserSignedIn) {
        if (mUserId != null) {

        }
    }

This makes statements easier to read and removes the unnecessary extra lines from the nested clauses.

#### 2.2.7.4 Ternary Operators

Where appropriate, ternary operators can be used to simplify operations.

For example, this is easy to read:


    userStatusImage = mSignedIn ? R.drawable.ic_tick : R.drawable.ic_cross;

and takes up far fewer lines of code than this:


    if (mSignedIn) {
        userStatusImage = R.drawable.ic_tick;
    } else {
        userStatusImage = R.drawable.ic_cross;
    }

**Note:** There are some times when ternary operators should not be used. If the if-clause logic is complex or a large number of characters then a standard brace style should be used.


### 2.2.8 Annotations

#### 2.2.8.1 Annotation practices

Taken from  the Android code style guide:

* `@Override`: The @Override annotation **must be used** whenever a method overrides the declaration or implementation from a super-class. For example, if you use the @inheritdocs Javadoc tag, and derive from a class (not an interface), you must also annotate that the method @Overrides the parent class's method.


* `@SuppressWarnings`: The @SuppressWarnings annotation should only be used under circumstances where it is impossible to eliminate a warning. If a warning passes this "impossible to eliminate" test, the @SuppressWarnings annotation must be used, so as to ensure that all warnings reflect actual problems in the code.

More information about annotation guidelines can be found [here](http://source.android.com/source/code-style.html#use-standard-java-annotations).

#### 2.2.8.2 Annotation style

Annotations that are applied to a method or class should always be defined in the declaration, with only one per line:


    @Annotation
    @AnotherAnnotation
    public class SomeClass {

      @SomeAnotation
      public String getMeAString() {

      }

    }

When using the annotations on fields, you should ensure that the annotation remains on the same line whilst there is room. For example:


    @Bind(R.id.layout_coordinator) CoordinatorLayout mCoordinatorLayout;


    @Inject MainPresenter mMainPresenter;


We do this as it makes the statement easier to read. For example, the statement '@Inject SomeComponent mSomeName' reads as 'inject this component with this name'.
    

#### 2.2.9 Limit variable scope

_The scope of local variables should be kept to a minimum (Effective Java Item 29). By doing so, you increase the readability and maintainability of your code and reduce the likelihood of error. Each variable should be declared in the innermost block that encloses all uses of the variable._

_Local variables should be declared at the point they are first used. Nearly every local variable declaration should contain an initializer. If you don't yet have enough information to initialize a variable sensibly, you should postpone the declaration until you do._ - ([Android code style guidelines](https://source.android.com/source/code-style.html#limit-variable-scope))


#### 2.2.10 Unused elements

All unused **fields**, **imports**, **methods** and **classes** should be removed from the code base unless there is any specific reasoning behind keeping it there.

#### 2.2.11 Order Import Statements

Because we use Android Studio, so imports should always be ordered automatically. However, in the case that they may not be, then they should be ordered as follows:


1. Android imports
2. Imports from third parties
3. java and javax imports
4. Imports from the current Project

**Note:**

- Imports should be alphabetically ordered within each grouping, with capital letters before lower case letters (e.g. Z before a)
- There should be a blank line between each major grouping (android, com, JUnit, net, org, java, javax)

#### 2.2.12 Logging

Logging should be used to log useful error messages and/or other information that may be useful during development.


| Log                               | Reason      |
|-----------------------------------|-------------|
| Log.v(String tag, String message) | verbose     |
| Log.d(String tag, String message) | debug       |
| Log.i(String tag, String message) | information |
| Log.w(String tag, String message) | warning     |
| Log.e(String tag, String message) | error       |


set the `Tag` for the log as a `static final` field at the top of the class, for example:


    private static final String TAG = MyActivity.class.getName();

All verbose and debug logs must be disabled on release builds. On the other hand - information, warning and error logs should only be kept enabled if deemed necessary.


    if (BuildConfig.DEBUG) {
        Log.d(TAG, "Here's a log message");
    }

**Note:** [Timber](https://github.com/JakeWharton/timber) is the preferred logging method to be used. It handles the tagging for us, which saves us keeping a reference to a TAG.

#### 2.2.13 Field Ordering

Any fields declared at the top of a class file should be ordered in the following order:

1. Enums
2. Constants
3. Dagger Injected fields
4. Butterknife View Bindings
5. private global variables
6. public global variables

For example:

	public static enum {
		ENUM_ONE, ENUM_TWO
	}

	public static final String KEY_NAME = "KEY_NAME";
	public static final int COUNT_USER = 0;

	@Inject SomeAdapter someAdapter;

	@BindView(R.id.text_name) TextView nameTextView;
	@BindView(R.id.image_photo) ImageView photoImageView;

	private int mUserCount;
	private String mErrorMessage;

	public int someCount;
	public String someString;

Using this ordering convention helps to keep field declarations grouped, which increases both the locating of and readability of said fields.

#### 2.2.14 Class member ordering


To improve code readability, it’s important to organise class members in a logical manner. The following order should be used to achieve this:


1. Constants
2. Fields
3. Constructors
4. Override methods and callbacks (public or private)
5. Public methods
6. Private methods
7. Inner classes or interfaces

For example:


    public class MainActivity extends Activity {

        private int mCount;

        public static newInstance() { }

        @Override
        public void onCreate() { }

        public setUsername() { }

        private void setupUsername() { }

        static class AnInnerClass { }

        interface SomeInterface { }

    }

Any lifecycle methods used in Android framework classes should be ordered in the corresponding lifecycle order. For example:


    public class MainActivity extends Activity {

        // Field and constructors

        @Override
        public void onCreate() { }

        @Override
        public void onStart() { }

        @Override
        public void onResume() { }

        @Override
        public void onPause() { }

        @Override
        public void onStop() { }

        @Override
        public void onRestart() { }

        @Override
        public void onDestroy() { }

        // public methods, private methods, inner classes and interfaces

    }

#### 2.2.15 Method parameter ordering

When programming for Android, it is quite common to define methods that take a `Context`. If you are writing a method like this, then the **Context** must be the **first** parameter.

The opposite case are **callback** interfaces that should always be the **last** parameter.

Examples:

```java
// Context always goes first
public User loadUser(Context context, int userId);

// Callbacks always go last
public void loadUserAsync(Context context, int userId, UserCallback callback);
```

#### 2.2.16 String constants, naming, and values

When using string constants, they should be declared as `static final` fields and they should be prefixed as indicated below.

| Element            | Field Name Prefix |
| -----------------  | ----------------- |
| SharedPreferences  | `PREF_`           |
| Bundle             | `BUNDLE_`         |
| Fragment Arguments | `ARGUMENT_`       |
| Intent Extra       | `EXTRA_`          |
| Intent Action      | `ACTION_`         |

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

#### 2.2.17 Enums

Enums should only be used where actually required. If another method is possible, then that should be the preferred way of approaching the implementation. For example:

Instead of this:


    public enum SomeEnum {
        ONE, TWO, THREE
    }

Do this:

    private static final int VALUE_ONE = 1;
    private static final int VALUE_TWO = 2;
    private static final int VALUE_THREE = 3;

#### 2.2.18 Arguments in fragments and activities

When creating new instances of a fragment or activity that involves passing data, we should provide a static method to retrieve the new instance, passing the data as method parameters. For example:

**Activity**

    public static Intent getStartIntent(Context context, Post post) {
        Intent intent = new Intent(context, CurrentActivity.class);
        intent.putParcelableExtra(EXTRA_POST, post);
        return intent;
    }

**Fragment**

    public static PostFragment newInstance(Post post) {
        PostFragment fragment = new PostFragment();
        Bundle args = new Bundle();
        args.putParcelable(ARGUMENT_POST, post);
        fragment.setArguments(args)
        return fragment;
    }
    
 **Note 1**: These methods should go at the top of the class before `onCreate()`.

**Note 2**: If we provide the methods described above, the keys for extras and arguments should be `private` because there is not need for them to be exposed outside the class.

#### 2.2.19 Line Length Limit

Code lines should exceed no longer than 100 characters, this makes the code more readable. Sometimes to achieve this, we may need to:


- Extract data to a local variable
- Extract logic to an external method
- Line-wrap code to separate a single line of code to multiple lines

**Note:** For code comments and import statements it’s ok to exceed the 100 character limit.

#### 2.2.19.1 Line-wrapping techniques

When it comes to line-wraps, there’s a few situations where we should be consistent in the way we format code.

**Breaking at Operators**

When we need to break a line at an operator, we break the line before the operator:


    int count = countOne + countTwo - countThree + countFour * countFive - countSix
            + countOnANewLineBecauseItsTooLong;

If desirable, you can always break after the `=` sign:


    int count =
            countOne + countTwo - countThree + countFour * countFive + countSix;

**Method Chaining**

When it comes to method chaining, each method call should be on a new line.

Don’t do this:


    Picasso.with(context).load("someUrl").into(imageView);

Instead, do this:


    Picasso.with(context)
            .load("someUrl")
            .into(imageView);

**Long Parameters**

In the case that a method contains long parameters, we should line break where appropriate. For example when declaring a method we should break after the last comma of the parameter that fits:


    private void someMethod(Context context, String someLongStringName, String text,
                                long thisIsALong, String anotherString) {               
    }             

And when calling that method we should break after the comma of each parameter:


    someMethod(context,
            "thisIsSomeLongTextItsQuiteLongIsntIt",
            "someText",
            01223892365463456,
            "thisIsSomeLongTextItsQuiteLongIsntIt");


#### 2.2.20 Method spacing

There only needs to be a single line space between methods in a class, for example:

Do this:


    public String getUserName() {
        // Code
    }

    public void setUserName(String name) {
        // Code
    }

    public boolean isLoggedIn() {
        // Code
    }

Not this:


    public String getUserName() {
        // Code
    }


    public void setUserName(String name) {
        // Code
    }


    public boolean isLoggedIn() {
        // Code
    }
    

### 2.2.22 Comments

#### 2.2.22.1 Inline comments

Where necessary, inline comments should be used to provide a meaningful description to the reader on what a specific piece of code does. They should only be used in situations where the code may be complex to understand. In most cases however, code should be written in a way that it easy to understand without comments.

**Note:** Code comments do not have to, but should try to, stick to the 100 character rule.

#### 2.2.22.2 JavaDoc Style Comments


Whilst a method name should usually be enough to communicate a methods functionality, it can sometimes help to provide JavaDoc style comments. This helps the reader to easily understand the methods functionality, as well as the purpose of any parameters that are being passed into the method.


    /**
     * Authenticates the user against the API given a User id.
     * If successful, this returns a success result
     *
     * @param userId The user id of the user that is to be authenticated.
     */

#### 2.2.22.3 Class comments

When creating class comments they should be meaningful and descriptive, using links where necessary. For example:


    /**
      * RecyclerView adapter to display a list of {@link Post}.
      * Currently used with {@link PostRecycler} to show the list of Post items.
      */

### 2.2.23 Butterknife

#### 2.2.23.1 Event listeners

Where possible, make use of Butterknife listener bindings. For example, when listening for a click event instead of doing this:


    mSubmitButton.setOnClickListener(new View.OnClickListener() {
        public void onClick(View v) {
            // Some code here...
        }
      };

Do this:


    @OnClick(R.id.button_submit)
    public void onSubmitButtonClick() { }


## 2.3 XML Style Rules

### 2.3.1 Use self=-closing tags

When a View in an XML layout does not have any child views, self-closing tags should be used.

Do:


    <ImageView
        android:id="@+id/image_user"
        android:layout_width="90dp"
        android:layout_height="90dp" />

Don’t:


    <ImageView
        android:id="@+id/image_user"
        android:layout_width="90dp"
        android:layout_height="90dp">
    </ImageView>


### 2.3.2 Resource naming

All resource names and IDs should be written using lowercase and underscores, for example:


    text_username, activity_main, fragment_user, error_message_network_connection

The main reason for this is consistency, it also makes it easier to search for views within layout files when it comes to altering the contents of the file.

#### 2.3.2.1 ID naming

All IDs should be prefixed using the name of the element that they have been declared for.

| Element        | Prefix    |
|----------------|-----------|
| ImageView      | image_    |
| Fragment       | fragment_ |
| RelativeLayout | layout_   |
| Button         | button_   |
| TextView       | text_     |
| View           | view_     |

For example:


    <TextView
        android:id="@+id/text_username"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />


Views that typically are only one per layout, such as a toolbar, can simply be given the id of it's view type. E.g.```toolbar```.

#### 2.3.2.2 Strings

All string names should begin with a prefix for the part of the application that they are being referenced from. For example:

| Screen                | String         | Resource Name             |
|-----------------------|----------------|---------------------------|
| Registration Fragment | “Register now” | registration_register_now |
| Sign Up Activity      | “Cancel”       | sign_up_cancel            |
| Rate App Dialog       | “No thanks”    | rate_app_no_thanks        |

If it’s not possible to name the referenced like the above, we can use the following rules:

| Prefix  | Description                                  |
|---------|----------------------------------------------|
| error_  | Used for error messages                      |
| title_  | Used for dialog titles                       |
| action_ | Used for option menu actions                 |
| msg_    | Used for generic message such as in a dialog |
| label_  | Used for activity labels                     |

Two important things to note for String resources:

 - String resources should never be reused across screens. This can cause issues when it comes to changing a string for a specific screen. It saves future complications by having a single string for each screens usage.

 - String resources should **always** be defined in the strings file and never hardcoded in layout or class files.

#### 2.3.2.3 Styles and themes

When defining both Styles & Themes, they should be named using UpperCamelCase. For example:


    AppTheme.DarkBackground.NoActionBar
    AppTheme.LightBackground.TransparentStatusBar

    ProfileButtonStyle
    TitleTextStyle


### 2.3.3 Attributes ordering

Ordering attributes not only looks tidy but it helps to make it quicker when looking for attributes within layout files. As a general rule,


1. View Id
2. Style
3. Layout width and layout height
4. Other `layout_` attributes, sorted alphabetically
5. Remaining attributes, sorted alphabetically

For example:

    <Button
        android:id="@id/button_accept"
        style="@style/ButtonStyle"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:layout_alignParentStart="true"
        android:padding="16dp"
        android:text="@string/button_skip_sign_in"
        android:textColor="@color/bluish_gray" />

Note: This formatting can be carried out by using the format feature in android studio -

`cmd + shift + L`

Doing this makes it easy to navigate through XML attributes when it comes to making changes to layout files.


# 3. Gradle Style
## 3.1 Dependencies

### 3.1.1 Versioning

Where applicable, versioning that is shared across multiple dependencies should be defined as a variable within the dependencies scope. For example:


    final SUPPORT_LIBRARY_VERSION = '23.4.0'

    compile "com.android.support:support-v4:$SUPPORT_LIBRARY_VERSION"
    compile "com.android.support:recyclerview-v7:$SUPPORT_LIBRARY_VERSION"
    compile "com.android.support:support-annotations:$SUPPORT_LIBRARY_VERSION"
    compile "com.android.support:design:$SUPPORT_LIBRARY_VERSION"
    compile "com.android.support:percent:$SUPPORT_LIBRARY_VERSION"
    compile "com.android.support:customtabs:$SUPPORT_LIBRARY_VERSION"

This makes it easy to update dependencies in the future as we only need to change the version number once for multiple dependencies.

### 3.1.2 Grouping

Where applicable, dependencies should be grouped by package name, with spaces in-between the groups. For example:


    compile "com.android.support:percent:$SUPPORT_LIBRARY_VERSION"
    compile "com.android.support:customtabs:$SUPPORT_LIBRARY_VERSION"

    compile 'io.reactivex:rxandroid:1.2.0'
    compile 'io.reactivex:rxjava:1.1.5'

    compile 'com.jakewharton:butterknife:7.0.1'
    compile 'com.jakewharton.timber:timber:4.1.2'

    compile 'com.github.bumptech.glide:glide:3.7.0'


`compile` , `testCompile` and `androidTestCompile`  dependencies should also be grouped into their corresponding section. For example:


    // App Dependencies
    compile "com.android.support:support-v4:$SUPPORT_LIBRARY_VERSION"
    compile "com.android.support:recyclerview-v7:$SUPPORT_LIBRARY_VERSION"

    // Instrumentation test dependencies
    androidTestCompile "com.android.support:support-annotations:$SUPPORT_LIBRARY_VERSION"

    // Unit tests dependencies
    testCompile 'org.robolectric:robolectric:3.0'

Both of these approaches makes it easy to locate specific dependencies when required as it keeps dependency declarations both clean and tidy 🙌


### 3.1.3 Independent Dependencies

Where dependencies are only used individually for application or test purposes, be sure to only compile them using `compile` , `testCompile` or `androidTestCompile` . For example, where the robolectric dependency is only required for unit tests, it should be added using:


    testCompile 'org.robolectric:robolectric:3.0'
    
    
    
## Credits    

We are extremely thankful to [Ribot](https://github.com/ribot) and [BufferApp](https://github.com/bufferapp) for providing community with best practices and android coding guidlines.

This guideline is inspired from:
1. [Ribot's Android Guideline](https://github.com/ribot/android-guidelines/blob/master/project_and_code_guidelines.md)
2. [BufferApp's Android Guideline](https://github.com/bufferapp/android-guidelines/blob/master/project_style_guidelines.md)

We in SquareBoat follow both above mentioned guideline and some of our own techniques.


