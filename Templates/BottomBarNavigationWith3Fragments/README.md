#Bottom bar navigation
![Bottom bar](https://github.com/CopyAndPasteHub/Android/blob/master/Templates/BottomBarNavigationWith3Fragments/gif/bottombar.gif)

##Credit
* [BottomBar by roughike](https://github.com/roughike/BottomBar)
* [FragNav by ncapdevi](https://github.com/ncapdevi/FragNav)

##Steps
1. Add dependencies in build.gradle
2. Add BottomBar and FragNavController in MainActivity.java
3. Add container in activity_main.xml
4. Create and add item for bottombar_menu.xml in res/menu
5. Create and add fragments

###Dependencies
Build.gradle
```
    compile 'com.ncapdevi:frag-nav:1.0.3'
    compile 'com.roughike:bottom-bar:1.3.9'
```

###MainActivity.java
```Java
public class MainActivity extends AppCompatActivity {
    private BottomBar mBottomBar;
    private FragNavController fragNavController;

    //indices to fragments
    private final int TAB_FIRST = FragNavController.TAB1;
    private final int TAB_SECOND = FragNavController.TAB2;
    private final int TAB_THIRD = FragNavController.TAB3;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        //FragNav
        //list of fragments
        List<Fragment> fragments = new ArrayList<>(3);

        //add fragments to list
        fragments.add(FirstFragment.newInstance(0));
        fragments.add(SecondFragment.newInstance(0));
        fragments.add(ThirdFragment.newInstance(0));

        //link fragments to container
        fragNavController = new FragNavController(getSupportFragmentManager(),R.id.container,fragments);
        //End of FragNav

        //BottomBar menu
        mBottomBar = BottomBar.attach(this, savedInstanceState);
        mBottomBar.setItems(R.menu.bottombar_menu);
        mBottomBar.setOnMenuTabClickListener(new OnMenuTabClickListener() {
            @Override
            public void onMenuTabSelected(@IdRes int menuItemId) {
                //switch between tabs
                switch (menuItemId) {
                    case R.id.bottomBarItemOne:
                        fragNavController.switchTab(TAB_FIRST);
                        break;
                    case R.id.bottomBarItemSecond:
                        fragNavController.switchTab(TAB_SECOND);
                        break;
                    case R.id.bottomBarItemThird:
                        fragNavController.switchTab(TAB_THIRD);
                        break;
                }
            }

            @Override
            public void onMenuTabReSelected(@IdRes int menuItemId) {
                if (menuItemId == R.id.bottomBarItemOne) {
                    fragNavController.clearStack();
                }
            }
        });
        //End of BottomBar menu

    }
    @Override
    public void onBackPressed() {
        if (fragNavController.getCurrentStack().size() > 1) {
            fragNavController.pop();
        } else {
            super.onBackPressed();
        }
    }
    @Override
    protected void onSaveInstanceState(Bundle outState) {
        super.onSaveInstanceState(outState);

        // Necessary to restore the BottomBar's state, otherwise we would
        // lose the current tab on orientation change.
        mBottomBar.onSaveInstanceState(outState);
    }
}
```

###activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:id="@+id/container">
</FrameLayout>
```

###bottombar_menu.xml
in /res/menu/
```xml
<?xml version="1.0" encoding="utf-8"?>
<!--Bottom bar menu by roughike-->
<!--https://github.com/roughike/BottomBar-->
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <!--Every item for another tab-->
    <item
        android:id="@+id/bottomBarItemOne"
        android:icon="@drawable/ic_looks_one_black_24dp"
        android:title="First item" />
    <item
        android:id="@+id/bottomBarItemSecond"
        android:icon="@drawable/ic_looks_two_black_24dp"
        android:title="Second item" />
    <item
        android:id="@+id/bottomBarItemThird"
        android:icon="@drawable/ic_looks_3_black_24dp"
        android:title="Third item" />
</menu>
```
