# MaterialSeachView
 MaterialSearchView là một thư viện rất hay về SeachView .Làm việc từ API Android 14 (ICS) trở lên. 
 # Sử dụng thêm dependencies vào file gradle của bạn    
 dependencies {         
        compile 'com.miguelcatalan:materialsearchview:1.4.0'
   }  
   
   thêm MaterialSearchView layout filecủa bạn dọc theo cùng thanh công cụ Toolbar
   <!— Must be last for right layering display —> 
    <!— Must be last for right layering display —>
    <FrameLayout
        android:id="@+id/toolbar_container"
        android:layout_width="match_parent"
        android:layout_height="wrap_content">

        <android.support.v7.widget.Toolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            android:background="@color/theme_primary" />

        <com.miguelcatalan.materialsearchview.MaterialSearchView
            android:id="@+id/search_view"
            android:layout_width="match_parent"
            android:layout_height="wrap_content" />
    </FrameLayout>
    
    thêm search item trong the menu file:     
    <item
        android:id="@+id/action_search"
        android:icon="@drawable/ic_action_action_search"
        android:orderInCategory="100"
        android:title="@string/abc_search_hint"
        app:showAsAction="always" />
        
   xét sự kiện listeners:     
   @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.menu_main, menu);

        MenuItem item = menu.findItem(R.id.action_search);
        searchView.setMenuItem(item);

        return true;
    }
   
   searchView.setOnSearchViewListener(new MaterialSearchView.SearchViewListener() {
            @Override
            public void onSearchViewShown() {
                //Do some magic
            }

            @Override
            public void onSearchViewClosed() {
                //Do some magic
            }
        });
        
  # Sử dụng VoiceSearch  cho phép/tắt nó trong code:      
        searchView.setVoiceSearch(true); //or false
        
  Xử lí trả về :      @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        if (requestCode == MaterialSearchView.REQUEST_VOICE && resultCode == RESULT_OK) {
            ArrayList<String> matches = data.getStringArrayListExtra(RecognizerIntent.EXTRA_RESULTS);
            if (matches != null && matches.size() > 0) {
                String searchWrd = matches.get(0);
                if (!TextUtils.isEmpty(searchWrd)) {
                    searchView.setQuery(searchWrd, false);
                }
            }

            return;
        }
        super.onActivityResult(requestCode, resultCode, data);
    }
    
  thêm suggestions  Xác định chúng trong các tài nguyên như một string-array:    <string-array name="query_suggestions">
        <item>Android</item>
        <item>iOS</item>
        <item>SCALA</item>
        <item>Ruby</item>
        <item>JavaScript</item>
    </string-array>		
    
Thêm chúng vào object:      
        searchView.setSuggestions(getResources().getStringArray(R.array.query_suggestions)); 		
       
 Style của nó!      <style name="MaterialSearchViewStyle">
        <!-- Background for the search bar -->
        <item name="searchBackground">@color/theme_primary</item>

        <!-- Change voice icon -->
        <item name="searchVoiceIcon">@drawable/ic_action_voice_search_inverted</item>

        <!-- Change clear text icon -->
        <item name="searchCloseIcon">@drawable/ic_action_navigation_close_inverted</item>

        <!-- Change up icon -->
        <item name="searchBackIcon">@drawable/ic_action_navigation_arrow_back_inverted</item>

        <!-- Change icon for the suggestions -->
        <item name="searchSuggestionIcon">@drawable/ic_suggestion</item>

        <!-- Change background for the suggestions list view -->
        <item name="searchSuggestionBackground">@android:color/white</item>

        <!-- Change text color for edit text. This will also be the color of the cursor -->
        <item name="android:textColor">@color/theme_primary_text_inverted</item>

        <!-- Change hint text color for edit text -->
        <item name="android:textColorHint">@color/theme_secondary_text_inverted</item>

        <!-- Hint for edit text -->
        <item name="android:hint">@string/search_hint</item>
    </style>
    
 tùy chỉnh cursor  tạo trong drawable:      
 
    <shape xmlns:android="http://schemas.android.com/apk/res/android">
        <size android:width="2dp" />
        <solid android:color="@color/theme_primary" />
    </shape>		 
    
 Và thêm nó vào đối tượng:      
        searchView.setCursorDrawable(R.drawable.custom_cursor); 		 

Sử dụng AppBarLayout?  It is a little bit tricky but can be achieved using this:      
 <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/container"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <!— Irrelevant stuff —>
    <android.support.v4.view.ViewPager
        android:id="@+id/viewpager"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_below="@+id/appbarlayout"
        app:layout_behavior="@string/appbar_scrolling_view_behavior" />

    <!— Must be last for right layering display —>
    <android.support.design.widget.AppBarLayout
        android:id="@+id/appbarlayout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@color/search_layover_bg">

        <FrameLayout
            android:id="@+id/toolbar_container"
            android:layout_width="match_parent"
            android:layout_height="wrap_content">

            <android.support.v7.widget.Toolbar
                android:id="@+id/toolbar"
                android:layout_width="match_parent"
                android:layout_height="?attr/actionBarSize"
                android:background="@color/theme_primary" />

            <com.miguelcatalan.materialsearchview.MaterialSearchView
                android:id="@+id/search_view"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:visibility="gone" />
        </FrameLayout>

        <android.support.design.widget.TabLayout
            android:id="@+id/tabs"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:background="@color/theme_primary"
            app:tabGravity="fill"
            app:tabMode="fixed" />

    </android.support.design.widget.AppBarLayout>

</RelativeLayout>
Bonus 
Close on backpressed:      
        @Override     
        public void onBackPressed() {         
                if (searchView.isSearchOpen()) {             
                        searchView.closeSearch();         
                   } else {
                        super.onBackPressed();
                      }     
        }
