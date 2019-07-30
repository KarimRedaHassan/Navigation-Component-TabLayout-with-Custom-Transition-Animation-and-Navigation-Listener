# Navigation-Component-TabLayout-with-Custom-Transition-Animation-and-Custom-Navigation-Listener
This Tutorial will discuss how to implement custom transition animation when using Navigation Component provided by Android Jetpack and TabLayout.

# Prerequisite:
This tutorial assumes that you are familiar with the following:
1. Android Jetpack Navigation Component
2. TabLayout

# Discussion
If you are familiar with Android Jetpack Navigation Component, you will find that there is no direct implementation with the TabLayout. Actually, The Navigation Component intentionally doesn't support direct implementation with TabLayout as "Navigation focuses on elements that affect the back stack and tabs do not affect the back stack - you should continue to manage tabs with a ViewPager and TabLayout" Google Issue Tracker. 
 
- https://issuetracker.google.com/issues/122087752

Here is How you could manage Tabs with ViewPager
- https://developer.android.com/guide/navigation/navigation-swipe-view

#### But What if you still want to implement any of the following case scenarios:
1. Navigation Component with TabLayout
2. Applying Custom Transition Animation

# Case Scenario One
For the first case scenario, you may have 5 tabs in the TabLayout to navigate between available destinations. We will supposed that you have created the TabLayout in the XML file as below

            <com.google.android.material.tabs.TabLayout
                android:id="@+id/tab_layout"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                app:tabMode="scrollable">

                <com.google.android.material.tabs.TabItem
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="Home" />

                <com.google.android.material.tabs.TabItem
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="Request" />

                <com.google.android.material.tabs.TabItem
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="Chat" />

                <com.google.android.material.tabs.TabItem
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="Notification" />

                <com.google.android.material.tabs.TabItem
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="Profile" />
            </com.google.android.material.tabs.TabLayout>


So, Here is the Steps to achieve the desired behavior in the first case scenario;
1. Create a NavOption object. You have to make sure of creating only one instance of the same destination target (the destination fragment you are navigating to) by adding this parameter to the NavOption Object

                .setLaunchSingleTop(true)
                
2. You will also need to make sure that the navigation BackStack is cleared with every transition so that when you press the back button, you go for the start destination fragment, not the previous selected fragment. This can be achieved by adding the following parameters to the NavOption Object

                .setPopUpTo(navController.getGraph().getStartDestination(), false)

3. Pass the NavOption Object to the NavController.navigate() method to be applied for the navigation operation
                    navController.navigate(item.getItemId(), null, navOptions);

4. Override the addOnTabSelectedListener() method to control what happen when you select a tab
5. Create s Switch/Case Statement to differentiate between different tabs based on their positions
6. Navigate to the desired destination fragment based on the tab position and don't forget to pass your NavOptions object.
7. Override the onBackPressed() method to check the tab related to the start destination fragment.

Here is a complete code snippet

    @Override
    protected void onCreate(Bundle savedInstanceState) {
    ....
        NavController navController = Navigation.findNavController(this, R.id.nav_host_fragment);
            
        NavOptions navOptions = new NavOptions.Builder()
                .setLaunchSingleTop(true)
                .setPopUpTo(navController.getGraph().getStartDestination(), false)
                .build();

        tabLayout.addOnTabSelectedListener(new TabLayout.OnTabSelectedListener() {
            @Override
            public void onTabSelected(TabLayout.Tab tab) {
                switch (tab.getPosition()) {
                    case 0:
                        navController.navigate(R.id.homeFragment, null, navOptions);
                        break;
                    case 1:
                        navController.navigate(R.id.requestsFragment, null, navOptions);
                        break;
                    case 2:
                        navController.navigate(R.id.chatFragment, null, navOptions);
                        break;
                    case 3:
                        navController.navigate(R.id.notificationsFragment, null, navOptions);
                        break;
                    case 4:
                        navController.navigate(R.id.profileFragment, null, navOptions);
                        break;
                }
            }

            @Override
            public void onTabUnselected(TabLayout.Tab tab) {

            }

            @Override
            public void onTabReselected(TabLayout.Tab tab) {

            }
        });
    }

    @Override
    public void onBackPressed() {
        tabLayout.getTabAt(0).select();
        super.onBackPressed(); 
    }

# Case Scenario Two
For this case scenario, you have the exact first scenario but you also want to apply a custom transition animation.

Here is the Steps to achieve the desired behavior in the Second case scenario;
1. Create a NavOption object and pass the custom transition animation to it using the following methods

                .setEnterAnim(R.anim.slide_in_right)
                .setExitAnim(R.anim.slide_out_left)
                .setPopEnterAnim(R.anim.slide_in_right)
                .setPopExitAnim(R.anim.slide_out_left)
        
2. Create a NavOption object. You have to make sure of creating only one instance of the same destination target (the destination fragment you are navigating to) by adding this parameter to the NavOption Object

                .setLaunchSingleTop(true)
                
3. You will also need to make sure that the navigation BackStack is cleared with every transition so that when you press the back button, you go for the start destination fragment, not the previous selected fragment. This can be achieved by adding the following parameters to the NavOption Object

                .setPopUpTo(navController.getGraph().getStartDestination(), false)

4. Pass the NavOption Object to the NavController.navigate() method to be applied for the navigation operation
                    navController.navigate(item.getItemId(), null, navOptions);

5. Override the addOnTabSelectedListener() method to control what happen when you select a tab
6. Create s Switch/Case Statement to differentiate between different tabs based on their positions
7. Navigate to the desired destination fragment based on the tab position and don't forget to pass your NavOptions object.
8. Override the onBackPressed() method to check the tab related to the start destination fragment.

Here is a complete code snippet

    @Override
    protected void onCreate(Bundle savedInstanceState) {
    ....
        NavController navController = Navigation.findNavController(this, R.id.nav_host_fragment);
            
        NavOptions navOptions = new NavOptions.Builder()
                .setLaunchSingleTop(true)
                .setEnterAnim(R.anim.slide_in_right)
                .setExitAnim(R.anim.slide_out_left)
                .setPopEnterAnim(R.anim.slide_in_right)
                .setPopExitAnim(R.anim.slide_out_left)
                .setPopUpTo(navController.getGraph().getStartDestination(), false)
                .build();

        tabLayout.addOnTabSelectedListener(new TabLayout.OnTabSelectedListener() {
            @Override
            public void onTabSelected(TabLayout.Tab tab) {
                switch (tab.getPosition()) {
                    case 0:
                        navController.navigate(R.id.homeFragment, null, navOptions);
                        break;
                    case 1:
                        navController.navigate(R.id.requestsFragment, null, navOptions);
                        break;
                    case 2:
                        navController.navigate(R.id.chatFragment, null, navOptions);
                        break;
                    case 3:
                        navController.navigate(R.id.notificationsFragment, null, navOptions);
                        break;
                    case 4:
                        navController.navigate(R.id.profileFragment, null, navOptions);
                        break;
                }
            }

            @Override
            public void onTabUnselected(TabLayout.Tab tab) {

            }

            @Override
            public void onTabReselected(TabLayout.Tab tab) {

            }
        });
    }

    @Override
    public void onBackPressed() {
        tabLayout.getTabAt(0).select();
        super.onBackPressed(); 
    }


# Worthy to be mentioned
If you apply this tutorial, You may achieve your desired behavior but you will violate two intended behavior
1. Cross fade animation between BottomNavigationView items is an intended behavior to follow the Material GuideLines.
2. Navigation focuses on elements that affect the back stack and tabs do not affect the back stack.

- https://issuetracker.google.com/issues/138665563
- https://issuetracker.google.com/issues/122087752

# What's Next ?

Navigation-Component-BottomNavigationView-with-Custom-Transition-Animation-and-Navigation-Listener

https://github.com/KarimRedaHassan/Navigation-Component-BottomNavigationView-with-Custom-Transition-Animation-and-Navigation-Listener

Navigation-Component-DrawerLayout-with-Custom-Transition-Animation-and-Custom-Navigation-Listener

https://github.com/KarimRedaHassan/Navigation-Component-DrawerLayout-with-Custom-Transition-Animation-and-Custom-Navigation-Listener

Navigation-Component-OptionsMenu-with-Custom-Transition-Animation-and-Custom-Navigation-Listener

https://github.com/KarimRedaHassan/Navigation-Component-OptionsMenu-with-Custom-Transition-Animation-and-Custom-Navigation-Listener

# Additional Resources

https://developer.android.com/guide/navigation/navigation-getting-started

https://developer.android.com/guide/navigation/navigation-ui

https://codelabs.developers.google.com/codelabs/android-navigation/index.html#0

https://developer.android.com/guide/navigation/navigation-swipe-view

# Also See

#### A Full List Of All My Tutorials

https://github.com/KarimRedaHassan?tab=repositories

