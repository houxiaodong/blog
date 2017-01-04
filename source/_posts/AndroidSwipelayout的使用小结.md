---
title: AndroidSwipelayout的使用小结
date: 2017-01-04 14:46:59
categories: Android
tags: Android 
---
# AndroidSwipelayout的使用小结 #
---

> 公司项目最近有用到侧滑删除的功能，网上找了很多开源控件，还是AndroidSwipelayout使用比较方便，自定义程度高，所以打算记录下使用过程，以及使用过程中踩的坑。

## 第一步引包 ##
> 从github 上引入包

	dependencies {
	    compile 'com.android.support:recyclerview-v7:21.0.0'
	    compile 'com.android.support:support-v4:20.+'
	    compile "com.daimajia.swipelayout:library:1.2.0@aar"
	}

## 第二步创建SwipeLayout布局 ##
> 在XML上创建SwipeLayout的布局

	<com.daimajia.swipe.SwipeLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    android:layout_width="match_parent" android:layout_height="80dp">
	    <!-- Bottom View Start-->
	     <LinearLayout
	        android:background="#66ddff00"
	        android:id="@+id/bottom_wrapper"
	        android:layout_width="160dp"
	        android:weightSum="1"
	        android:layout_height="match_parent">
	        <!--What you want to show-->
	    </LinearLayout>
	    <!-- Bottom View End-->
	
	    <!-- Surface View Start -->
	     <LinearLayout
	        android:padding="10dp"
	        android:background="#ffffff"
	        android:layout_width="match_parent"
	        android:layout_height="match_parent">
	        <!--What you want to show in SurfaceView-->
	    </LinearLayout>
	    <!-- Surface View End -->
	</com.daimajia.swipe.SwipeLayout>


Surface View 是你想要展示在外面的布局，Bottom View是你想隐藏在后面的布局，用来滑动脱拖出的布局。


## 第三步初始化SwipeLayout布局 ##
> 初始化你的SwipeLayout布局，并且设置监听，处理事件。

	SwipeLayout swipeLayout =  (SwipeLayout)findViewById(R.id.sample1);
	
	//set show mode.
	swipeLayout.setShowMode(SwipeLayout.ShowMode.LayDown);
	
	//add drag edge.(If the BottomView has 'layout_gravity' attribute, this line is unnecessary)
	swipeLayout.addDrag(SwipeLayout.DragEdge.Left, findViewById(R.id.bottom_wrapper));
	
	swipeLayout.addSwipeListener(new SwipeLayout.SwipeListener() {
	            @Override
	            public void onClose(SwipeLayout layout) {
	                //when the SurfaceView totally cover the BottomView.
	            }
	
	            @Override
	            public void onUpdate(SwipeLayout layout, int leftOffset, int topOffset) {
	               //you are swiping.
	            }
	
	            @Override
	            public void onStartOpen(SwipeLayout layout) {
	
	            }
	
	            @Override
	            public void onOpen(SwipeLayout layout) {
	               //when the BottomView totally show.
	            }
	
	            @Override
	            public void onStartClose(SwipeLayout layout) {
	
	            }
	
	            @Override
	            public void onHandRelease(SwipeLayout layout, float xvel, float yvel) {
	               //when user's hand released.
	            }
	        });  
## 第四步设置数据 ##

SwipeLayout 提供了SwipeAdapter给布局做数据适配，它一共封装了4种数据适配：BaseSwipeAdapter、ArraySwipeAdapter、CursorSwipeAdapter、SimpleCursorSwipeAdapter。  
下面以BaseSwipeAdapter举例：  

	public class AddListViewSwipeAdapter extends BaseSwipeAdapter {
	    private Context mContext;
	    private LayoutInflater mInflater;
	    public List<String> mlist;
	
	    SwipeLayout swipeLayout;
	
	
	    public AddListViewSwipeAdapter(Context mContext, List<String> list) {
	        this.mContext = mContext;
	        mInflater = LayoutInflater.from(mContext);
	        this.mlist = list;
	    }
	
	    @Override
	    public int getSwipeLayoutResourceId(int position) {
	        return R.id.listview_swipe;
	    }
	
	    @Override
	    public View generateView(final int position, ViewGroup parent) {
	        View v = mInflater.inflate(R.layout.item_add_time, null);
	        swipeLayout = (SwipeLayout) v.findViewById(getSwipeLayoutResourceId(position));
	        swipeLayout.addSwipeListener(new SimpleSwipeListener() {
	            @Override
	            public void onOpen(SwipeLayout layout) {
	
	            }
	        });
	        swipeLayout.setOnDoubleClickListener(new SwipeLayout.DoubleClickListener() {
	            @Override
	            public void onDoubleClick(SwipeLayout layout, boolean surface) {
	                Toast.makeText(mContext, "DoubleClick", Toast.LENGTH_SHORT).show();
	            }
	        });
	        return v;
	    }
	
	    @Override
	    public void fillValues(final int position, View convertView) {
		//注意：这个里面的position才会是随item变化而递增的，处理list的数据要在这个方法中
	        Button button = (Button) convertView.findViewById(R.id.delete);
	        TextView tv_cishu = (TextView) convertView.findViewById(R.id.tv_cishu);
	        TextView tv_shijian = (TextView) convertView.findViewById(R.id.tv_shijian);
	        button.setText("删除");
	        tv_cishu.setText("第" + (position + 1) + "次");
	        tv_shijian.setText(mlist.get(position));
		//设置监听事件
	        swipeLayout.findViewById(R.id.delete).setOnClickListener(new View.OnClickListener() {
	            @Override
	            public void onClick(View view) {
	                mlist.remove(position);
	                for (SwipeLayout s : getOpenLayouts()
	                        ) {
	                    //关闭侧滑区域
	                    if (s.getOpenStatus() == SwipeLayout.Status.Open) {
	                        s.close();
	                    }
	                }
	                notifyDataSetChanged();
	            }
	        });
	
	    }
	
	    @Override
	    public int getCount() {
	        return mlist.size();
	    }
	
	    @Override
	    public Object getItem(int position) {
	        return mlist.get(position);
	    }
	
	    @Override
	    public long getItemId(int position) {
	        return position;
	    }
	
	    public List getList() {
	        return mlist;
	    }
	}

[感谢代码家提供的控件，以下是github的链接地址](https://github.com/daimajia/AndroidSwipeLayout)
