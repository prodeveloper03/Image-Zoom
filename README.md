# Image-Zoom
Image zoom aims to help produce an easily usable implementation of a zooming Android ImageView.



----------------------------------------------------
						 FEATURE DEMO
----------------------------------------------------  



https://user-images.githubusercontent.com/62813305/118403175-cca46500-b68a-11eb-8702-68bd6b0abdf5.mov



## Dependency

Add this in your root `build.gradle` file (**not** your module `build.gradle` file):

```gradle
allprojects {
    repositories {
        maven { url "https://www.jitpack.io" }
    }
}

buildscript {
    repositories {
        maven { url "https://www.jitpack.io" }
    }	
}
```

Then, add the library to your module `build.gradle`
```gradle
dependencies {
    implementation 'com.github.chrisbanes:PhotoView:2.0.0'
}
```

## Features
- Out of the box zooming, using multi-touch and double-tap.
- Scrolling, with smooth scrolling fling.
- Works perfectly when used in a scrolling parent (such as ViewPager).
- Allows the application to be notified when the displayed Matrix has changed. Useful for when you need to update your UI based on the current zoom/scroll position.
- Allows the application to be notified when the user taps on the Photo.

## Usage
There is a [sample](https://github.com/chrisbanes/PhotoView/tree/master/sample) provided which shows how to use the library in a more advanced way, but for completeness, here is all that is required to get PhotoView working:
```xml
<com.github.chrisbanes.photoview.PhotoView
    android:id="@+id/photo_view"
    android:layout_width="match_parent"
    android:layout_height="match_parent"/>
```
```java
PhotoView photoView = (PhotoView) findViewById(R.id.photo_view);
photoView.setImageResource(R.drawable.image);
```
That's it!

## Issues With ViewGroups
There are some ViewGroups (ones that utilize onInterceptTouchEvent) that throw exceptions when a PhotoView is placed within them, most notably [ViewPager](http://developer.android.com/reference/android/support/v4/view/ViewPager.html) and [DrawerLayout](https://developer.android.com/reference/android/support/v4/widget/DrawerLayout.html). This is a framework issue that has not been resolved. In order to prevent this exception (which typically occurs when you zoom out), take a look at [HackyDrawerLayout](https://github.com/chrisbanes/PhotoView/blob/master/sample/src/main/java/com/github/chrisbanes/photoview/sample/HackyDrawerLayout.java) and you can see the solution is to simply catch the exception. Any ViewGroup which uses onInterceptTouchEvent will also need to be extended and exceptions caught. Use the [HackyDrawerLayout](https://github.com/chrisbanes/PhotoView/blob/master/sample/src/main/java/com/github/chrisbanes/photoview/sample/HackyDrawerLayout.java) as a template of how to do so. The basic implementation is:
```java
public class HackyProblematicViewGroup extends ProblematicViewGroup {

    public HackyProblematicViewGroup(Context context) {
        super(context);
    }

    @Override
    public boolean onInterceptTouchEvent(MotionEvent ev) {
        try {
            return super.onInterceptTouchEvent(ev);
        } catch (IllegalArgumentException e) {
						//uncomment if you really want to see these errors
            //e.printStackTrace();
            return false;
        }
    }
}
```

## Usage with Fresco
Due to the complex nature of Fresco, this library does not currently support Fresco. See [this project](https://github.com/ongakuer/PhotoDraweeView) as an alternative solution.

## Subsampling Support
This library aims to keep the zooming implementation simple. If you are looking for an implementation that supports subsampling, check out [this project](https://github.com/davemorrissey/subsampling-scale-image-view)
