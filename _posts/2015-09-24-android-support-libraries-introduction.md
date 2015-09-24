##[Android Support Libraries][SupportFeatures]

####Support Libraries主要是对老版本系统提供新API的支持以及一些辅助工具类。


###一、Android v4 Support Libraries

	com.android.support:support-v4:21.0.0

1.v4库可以支持Android1.6(API 4)及以上系统版本，它包含的主要特性有：

*	**App Components**	
	+ [**Fragment**][Fragment] - Adds support for encapsulation of user interface and functionality with Fragments, enabling applications to provide layouts that adjust between small and large-screen devices.
	
	+ [**NotificationCompat**][NotificationCompat] - Adds support for rich notification features.
	
	+ [**LocalBroadcastManager**][LocalBroadcastManager] - Allows applications to easily register for and receive intents within a single application without broadcasting them globally.

*	**User Interface**

	+ [**ViewPager**][ViewPager] - Adds a [**ViewGroup**][ViewGroup] that manages the layout for the child views, which the user can swipe between.		
	
	+ [**PagerTitleStrip**][PagerTitleStrip] - Adds a non-interactive title strip, that can be added as a child of [**ViewPager**][ViewPager].	
	+ [**PagerTabStrip**][PagerTabStrip] - Adds a navigation widget for switching between paged views, that can also be used with [**ViewPager**][ViewPager].
	
	+ [**DrawerLayout**][DrawerLayout] - Adds support for creating a Navigation Drawer that can be pulled in from the edge of a window.		
	
	+ [**SlidingPaneLayout**][SlidingPaneLayout] - Adds widget for creating linked summary and detail views that appropriately adapt to various screen sizes.

* 	**Accessibility**

	+ [**ExploreByTouchHelper**][ExploreByTouchHelper] - Adds a helper class for implementing accessibility support for custom views.
	
	+ [**AccessibilityEventCompat**][AccessibilityEventCompat] - Adds support for [**AccessibilityEvent**][AccessibilityEvent]. For more information about implementing accessibility, see [Accessibility][Accessibility].
	
	+ [**AccessibilityNodeInfoCompat**][AccessibilityNodeInfoCompat] - Adds support for [**AccessibilityNodeInfo**][AccessibilityNodeInfo].
	
	+ [**AccessibilityNodeProviderCompat**][AccessibilityNodeProviderCompat] - Adds support for [**AccessibilityNodeProvider**][AccessibilityNodeProvider].
	
	+ [**AccessibilityDelegateCompat**][AccessibilityDelegateCompat] - Adds support for [**View.AccessibilityDelegate**][View.AccessibilityDelegate].

* 	**Content**

	+ [**Loader**][Loader] - Adds support for asynchronous loading of data. The library also provides concrete implementations of this class, including [**CursorLoader**][CursorLoader] and [**AsyncTaskLoader**][AsyncTaskLoader].
	
	+ [**FileProvider**][FileProvider] - Adds support for sharing of private files between applications.


2.常用API

- [**Fragment**][Fragment]

	使用v4的Fragment的话，Activity要继承FragmentActivity，同时要使用#getSupportFragmentManager()来获取FragmentManager。

		Tips:
		使用v7的话，Activity可以继承ActionBarActivity。

- [**ViewPager**][ViewPager]

	ViewPager建议和Fragment一起使用，这样可以独立管理ViewPager每一页的生命周期。
	



###二、Android v7 Support Libraries


   v7有些特性依赖于v4，所以用v7时最好引入v4。可以运行在Android2.1(API 7)及以上，它包含有多个单独的库，主要如下：

#####1.v7 appcompat library
		com.android.support:appcompat-v7:21.0.0

1.支持**Material Design** UI，主要是支持[**ActionBar**][ActionBar]。包含的关键类有：

* [**ActionBar**][ActionBar] - Provides an implementation of the action bar user interface pattern. For more information on using the Action Bar, see the [Action Bar][ActionBarGuide] developer guide.
 
* [**AppCompatActivity**][AppCompatActivity] - Adds an application activity class that can be used as a base class for activities that use the Support Library action bar implementation.

* [**AppCompatDialog**][AppCompatDialog] - Adds a dialog class that can be used as a base class for AppCompat themed dialogs.

* [**ShareActionProvider**][ShareActionProvider] - Adds support for a standardized sharing action (such as email or posting to social applications) that can be included in an action bar.

2.ActionBar的使用：

- 添加
	
		a)创建一个继承ActionBarActivity的Activity (ActionBarActivity已经被舍弃，建议继承AppCompatActivity)
		
		b)该Activity使用Theme.AppCompat或其子Theme

- 动态隐藏和显示

		getSupportActionBar()获取到ActionBar对象，通过调用hide()、show()来隐藏和显示。隐藏和显示都会重绘，如果频繁地隐藏显示，可以在设置theme的windowActionBarOverlay为true，则ActionBar会独立在顶层，不会占有内容的空间。
		
- 添加ActionView

	在Menu文件的item#actionViewClass属性设置对应的类。比如添加一个SearchView:
	
		<?xml version="1.0" encoding="utf-8"?>
		<menu xmlns:android="http://schemas.android.com/apk/res/android"
		      xmlns:yourapp="http://schemas.android.com/apk/res-auto" >
		    <item android:id="@+id/action_search"
		          android:title="@string/action_search"
		          android:icon="@drawable/ic_action_search"
		          yourapp:showAsAction="ifRoom|collapseActionView"
		          yourapp:actionViewClass="android.support.v7.widget.SearchView" />
		</menu>

	另外可以设置showAsAction属性为collapseActionView，这样ActionView就可以拉伸，通过[*OnActionExpandListener*][OnActionExpandListener]监听拉伸状态。
	
		@Override
		public boolean onCreateOptionsMenu(Menu menu) {
    		getMenuInflater().inflate(R.menu.options, menu);
    		MenuItem menuItem = menu.findItem(R.id.actionItem);
    		...

    		// When using the support library, the setOnActionExpandListener() method is
    		// static and accepts the MenuItem object as an argument
    		MenuItemCompat.setOnActionExpandListener(menuItem, new OnActionExpandListener() {
        		@Override
        		public boolean onMenuItemActionCollapse(MenuItem item) {
            		// Do something when collapsed
            		return true;  // Return true to collapse action view
        		}

        		@Override
        		public boolean onMenuItemActionExpand(MenuItem item) {
            		// Do something when expanded
            		return true;  // Return true to expand action view
        		}
    		});
		}

- 其他
	
	ActionProvider:
	
	![image][ActionProviderPic]
	
	Navigation Tabs:
	
	![image][NavigationTab1]
	![image][NavigationTab2]
	
	Drop-down Navigation:
	
	![image][DropDownPic]


#####2.v7 recyclerview library
		com.android.support:recyclerview-v7:21.0.0

[**RecyclerView**][RecyclerView]: 替代ListView、GridView的方案，更强大、更灵活易用。原生支持List、Grid、StaggeredGrid三种LayoutManager，原生不支持CursorAdapter、不支持OnItemClickListener。

使用：

1.设置LayoutManager

2.设置继承RecyclerView.Adapter的Adapter，泛型传入继承RecyclerView.ViewHolder的ViewHolder




#####3.v7 cardview library
		com.android.support:cardview-v7:21.0.0
		
[**CardView**][CardView]: 支持圆角背景和阴影的ViewGroup。

+ cardCornerRadius 设置圆角半径
+ cardBackgroundColor 设置背景
+ cardElevation 设置阴影大小（立体感）
+ contentPadding 设置内容Padding


#####4.v7 gridlayout library
		com.android.support:gridlayout-v7:21.0.0

[**GridLayout**][GridLayout]: 支持类表格布局的ViewGroup，比TabLayout更强大、更好用，可以指定行列、占多少行列等。


#####5.v7 palette library
		com.android.support:palette-v7:21.0.0
		
[**Palette**][Palette]:可以从图片中提取颜色给其他UI使用，现可以提取六种风格的颜色。这样UI可以根据不同的图片显示不同的颜色，更融洽灵活。

使用：

* 初始化有两种方法:
	- 同步：	
	
			Palette.generate(Bitmap)
			Palette.generate(Bitmap, int)
			
	- 异步：
	
			generateAsync(Bitmap, PaletteAsyncListener)
			generateAsync(Bitmap, int, PaletteAsyncListener)

* 获取[Swatch][Swatch]：
		
		Palette.Swatch swatch = palette.getDarkVibrantSwatch()

* 获取颜色：

		Swatch提供了几种颜色，可以直接拿来用。
		
		
#####6.v7 Preference Support Library
		com.android.support:preference-v7:23.0.0

[Preference][Preference]提供了一些常用的选项设置API，包括[**CheckBoxPreference**][CheckBoxPreference]、[**ListPreference**][ListPreference]、[**EditTextPreference**][EditTextPreference]等



###3.Percent Support Library
		com.android.support:percent:23.0.0
		
提供百分比布局支持。
[Demo][PercentDemo]

###4.Design Support Library
		com.android.support:design:22.2.0
		
提供一些Material Design的组件和UI，包括Navigation Drawers, Floating Action Buttons (FAB), Snackbars, and Tabs等。
	[Material Design介绍][Material Design Introduction]	
	
###5.Annotations Support Library
		com.android.support:support-annotations:22.0.0

提供可增强类型的注解，减少出错。比如对一个方法@WorkerThread，如果这个方法在主线程调用会编译异常。


###6.Multidex Support Library
		com.android.support:multidex:1.0.0
		
用来解决dex方法数大于65535问题。Dalvik限制一个App只能有一个dex文件，ART运行模式则没有限制。Multidex库就是用来支持App可以有多个dex文件的。[Multidex用法详情][BuildingAppWithOver65Methods]


[SupportFeatures]:https://developer.android.com/tools/support-library/features.html

[Fragment]:https://developer.android.com/reference/android/support/v4/app/Fragment.html

[NotificationCompat]:https://developer.android.com/reference/android/support/v4/app/NotificationCompat.html

[LocalBroadcastManager]:https://developer.ndroid.com/reference/android/support/v4/content/LocalBroadcastManager.html

[ViewPager]:https://developer.android.com/reference/android/support/v4/view/ViewPager.html

[ViewGroup]:https://developer.android.com/reference/android/view/ViewGroup.html

[PagerTitleStrip]:https://developer.android.com/reference/android/support/v4/view/PagerTitleStrip.html

[PagerTabStrip]:https://developer.android.com/reference/android/support/v4/view/PagerTabStrip.html

[DrawerLayout]:https://developer.android.com/reference/android/support/v4/widget/DrawerLayout.html

[SlidingPaneLayout]:https://developer.android.com/reference/android/support/v4/widget/SlidingPaneLayout.html

[ExploreByTouchHelper]:https://developer.android.com/reference/android/support/v4/widget/ExploreByTouchHelper.html

[AccessibilityEventCompat]:https://developer.android.com/reference/android/support/v4/view/accessibility/AccessibilityEventCompat.html

[AccessibilityEvent]:https://developer.android.com/reference/android/view/accessibility/AccessibilityEvent.html

[Accessibility]:https://developer.android.com/guide/topics/ui/accessibility/index.html

[AccessibilityNodeInfoCompat]:https://developer.android.com/reference/android/support/v4/view/accessibility/AccessibilityNodeInfoCompat.html

[AccessibilityNodeInfo]:https://developer.android.com/reference/android/view/accessibility/AccessibilityNodeInfo.html

[AccessibilityNodeProviderCompat]:https://developer.android.com/reference/android/support/v4/view/accessibility/AccessibilityNodeProviderCompat.html

[AccessibilityNodeProvider]:https://developer.android.com/reference/android/view/accessibility/AccessibilityNodeProvider.html

[AccessibilityDelegateCompat]:https://developer.android.com/reference/android/support/v4/view/AccessibilityDelegateCompat.html

[View.AccessibilityDelegate]:https://developer.android.com/reference/android/view/View.AccessibilityDelegate.html

[Loader]:https://developer.android.com/reference/android/support/v4/content/Loader.html

[CursorLoader]:https://developer.android.com/reference/android/support/v4/content/CursorLoader.html

[AsyncTaskLoader]:https://developer.android.com/reference/android/support/v4/content/AsyncTaskLoader.html

[FileProvider]:https://developer.android.com/reference/android/support/v4/content/FileProvider.html

[ActionBar]:https://developer.android.com/reference/android/support/v7/app/ActionBar.html
[ActionBarGuide]:https://developer.android.com/guide/topics/ui/actionbar.html

[AppCompatActivity]:https://developer.android.com/reference/android/support/v7/app/AppCompatActivity.html

[AppCompatDialog]:https://developer.android.com/reference/android/support/v7/app/AppCompatDialog.html

[ShareActionProvider]:https://developer.android.com/reference/android/support/v7/widget/ShareActionProvider.html

[OnActionExpandListener]:https://developer.android.com/reference/android/support/v4/view/MenuItemCompat.OnActionExpandListener.html

[ActionProviderPic]:./actionbar-shareaction.png

[NavigationTab1]:./actionbar-tabs1.png
[NavigationTab2]:./actionbar-tabs2.png
[DropDownPic]:./actionbar-dropdown.png

[CardView]: https://developer.android.com/reference/android/support/v7/widget/CardView.html

[RecyclerView]: https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html

[GridLayout]:https://developer.android.com/reference/android/support/v7/widget/GridLayout.html

[Palette]:https://developer.android.com/reference/android/support/v7/graphics/Palette.html

[Swatch]:https://developer.android.com/reference/android/support/v7/graphics/Palette.Swatch.html

[Preference]:https://developer.android.com/reference/android/support/v7/preference/package-summary.html

[CheckBoxPreference]:https://developer.android.com/reference/android/support/v7/preference/CheckBoxPreference.html

[ListPreference]:https://developer.android.com/reference/android/support/v7/preference/ListPreference.html

[EditTextPreference]:https://developer.android.com/reference/android/support/v7/preference/EditTextPreference.html

[PercentDemo]:https://github.com/JulienGenoud/android-percent-support-lib-sample

[Material Design Introduction]:https://www.google.com/design/spec/material-design/introduction.html

[BuildingAppWithOver65Methods]:https://developer.android.com/tools/building/multidex.html