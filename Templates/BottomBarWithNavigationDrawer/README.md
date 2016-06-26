#Drawer navigation with bottom bar
![Bottom bar](https://github.com/CopyAndPasteHub/Android/blob/master/Templates/BottomBarWithNavigationDrawer/gif/navdrawer.gif)

##Credit
* [MaterialDrawer by mikepenz](https://github.com/mikepenz/MaterialDrawer)
* [BottomBar by roughike](https://github.com/roughike/BottomBar)
* [FragNav by ncapdevi](https://github.com/ncapdevi/FragNav)

##Steps
1. Add dependencies in build.gradle
2. Add NavigationDrawer, BottomBar and FragNavController in MainActivity.java
3. Add container in activity_main.xml and include toolbar.xml
4. Create and add item for bottombar_menu.xml in res/menu
5. Create and add fragments, classes

###Dependencies
Build.gradle
```
    compile 'com.ncapdevi:frag-nav:1.0.3'
    compile 'com.roughike:bottom-bar:1.3.9'
    compile('com.mikepenz:materialdrawer:5.3.1@aar') {
        transitive = true
    }
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

        //Navigation drawer
        new DrawerBuilder().withActivity(this).build();

        //primary items
        PrimaryDrawerItem home = new PrimaryDrawerItem()
                .withIdentifier(1)
                .withName(R.string.drawer_item_home)
                .withIcon(R.drawable.ic_home_black_24dp);
        PrimaryDrawerItem primary_item1 = new PrimaryDrawerItem()
                .withIdentifier(2)
                .withName(R.string.drawer_item_option1)
                .withIcon(R.drawable.ic_looks_one_black_24dp);
        PrimaryDrawerItem primary_item2 = new PrimaryDrawerItem()
                .withIdentifier(3)
                .withName(R.string.drawer_item_option2)
                .withIcon(R.drawable.ic_looks_two_black_24dp);
        //secondary items
        SecondaryDrawerItem secondary_item1 = (SecondaryDrawerItem) new SecondaryDrawerItem()
                .withIdentifier(11)
                .withName(R.string.drawer_item_option1)
                .withIcon(R.drawable.ic_looks_one_black_24dp);
        SecondaryDrawerItem secondary_item2 = (SecondaryDrawerItem) new SecondaryDrawerItem()
                .withIdentifier(12)
                .withName(R.string.drawer_item_option2)
                .withIcon(R.drawable.ic_looks_two_black_24dp);
        SecondaryDrawerItem secondary_item3 = (SecondaryDrawerItem) new SecondaryDrawerItem()
                .withIdentifier(13)
                .withName(R.string.drawer_item_option3)
                .withIcon(R.drawable.ic_looks_3_black_24dp);
        //settings, help, contact items
        SecondaryDrawerItem settings = (SecondaryDrawerItem) new SecondaryDrawerItem()
                .withIdentifier(97)
                .withName(R.string.drawer_item_settings)
                .withIcon(R.drawable.ic_settings_black_24dp);
        SecondaryDrawerItem help = (SecondaryDrawerItem) new SecondaryDrawerItem()
                .withIdentifier(98)
                .withName(R.string.drawer_item_help)
                .withIcon(R.drawable.help);
        SecondaryDrawerItem contact = (SecondaryDrawerItem) new SecondaryDrawerItem()
                .withIdentifier(99)
                .withName(R.string.drawer_item_contact)
                .withIcon(R.drawable.ic_contact_mail_black_24dp);

        //Toolbar
        Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);
        getSupportActionBar().setDisplayHomeAsUpEnabled(true);
        getSupportActionBar().setHomeButtonEnabled(false);

        new DrawerBuilder()
                .withActivity(this)
                .withToolbar(toolbar)
                .withActionBarDrawerToggleAnimated(true)
                .withTranslucentStatusBar(false)
                .withFullscreen(true)
                .withSavedInstance(savedInstanceState)
                .addDrawerItems(
                        home,
                        primary_item1,
                        primary_item2,
                        new SectionDrawerItem().withName("Categories"),
                        secondary_item1,
                        secondary_item2,
                        secondary_item2,
                        new DividerDrawerItem(),
                        settings,
                        help,
                        contact

                )
                .withOnDrawerItemClickListener(new Drawer.OnDrawerItemClickListener() {
                    @Override
                    public boolean onItemClick(View view, int position, IDrawerItem drawerItem) {
                        if (drawerItem != null) {
                            Intent intent = null;
                            if (drawerItem.getIdentifier() == 1) {
                                intent = new Intent(MainActivity.this, MainActivity.class);
                            } else if (drawerItem.getIdentifier() == 2) {
                                //intent = new Intent(MainActivity.this, Class.class);
                            } else if (drawerItem.getIdentifier() == 3) {
                                //intent = new Intent(MainActivity.this, Class.class);
                            } else if (drawerItem.getIdentifier() == 11) {
                                //intent = new Intent(MainActivity.this, Class.class);
                            } else if (drawerItem.getIdentifier() == 12) {
                                //intent = new Intent(MainActivity.this, Class.class);
                            } else if (drawerItem.getIdentifier() == 13) {
                                //intent = new Intent(MainActivity.this, Class.class);
                            } else if (drawerItem.getIdentifier() == 97) {
                                intent = new Intent(MainActivity.this, Settings.class);
                            } else if (drawerItem.getIdentifier() == 98) {
                                intent = new Intent(MainActivity.this, Help.class);
                            } else if (drawerItem.getIdentifier() == 99) {
                                intent = new Intent(MainActivity.this, Contact.class);
                            }
                            if (intent != null) {
                                MainActivity.this.startActivity(intent);
                            }
                        }

                        return false;
                    }
                })
                .build();
        //End of Navigation drawer

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
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    tools:context=".MainActivity"
    android:orientation="vertical"
    android:layout_height="match_parent"
    android:layout_width="match_parent">
    <!--Include a toolbar-->
    <include
        layout="@layout/toolbar"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>
    <!--Container of fragments-->
    <FrameLayout
        android:id="@+id/container"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
    </FrameLayout>
</LinearLayout>
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

###toolbar.xml
int /res/layout/
```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v7.widget.Toolbar
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="?attr/actionBarSize"
    android:layout_gravity="center_horizontal|top"
    android:background="?attr/colorPrimary"
    android:elevation="4dp"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    app:popupTheme="@style/ThemeOverlay.AppCompat.Light"/>
```
