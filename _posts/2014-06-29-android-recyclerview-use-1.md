---
layout: post
category: android
title: android-RecyclerView 的使用（一）
description: ""
modified: 2014-06-29
tags: [android]
comments: true
share: true
comments: true
---

android-RecyclerView 的使用（一）
-
`RecyclerView` 是 android-support-v7-21 版本中新增的一个 Widgets, 还有一个 `CardView` 会在下次介绍使用。  
官方介绍 `RecyclerView` 是 `ListView` 的升级版本，更加先进和灵活。我们写一个简单的实例例，来看一下究竟有多先进和灵活。

## 开发环境
* IDE： AndroidStudio 0.8.0
* SDK： Android L

build.gradle 配置

{% highlight groovy %}
android {
    compileSdkVersion 'android-L'
    buildToolsVersion "20.0.0"

    defaultConfig {
        minSdkVersion 'L'
        targetSdkVersion 'L'
		...
    }
    ...
}

dependencies {
    compile 'com.android.support:recyclerview-v7:+'
	...
}
{% endhighlight %}

## 开始

首先是布局文件中使用 `RecyclerView` 

{% highlight xml %}
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MyActivity">

    <android.support.v7.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:scrollbars="vertical" />

</RelativeLayout>
{% endhighlight %}

Activity 中

{% highlight java %}
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_my);

        RecyclerView recyclerView = (RecyclerView) findViewById(R.id.recyclerView);

        // 创建一个线性布局管理器
        LinearLayoutManager layoutManager = new LinearLayoutManager(this);
        // 设置布局管理器
        recyclerView.setLayoutManager(layoutManager);

        // 创建数据集
        String[] dataset = new String[100];
        for (int i = 0; i < dataset.length; i++){
            dataset[i] = "item" + i;
        }
        // 创建Adapter，并指定数据集
        MyAdapter adapter = new MyAdapter(dataset);
        // 设置Adapter
        recyclerView.setAdapter(adapter);

    }
{% endhighlight %}

`RecyclerView` 首先的一个特点就是，将 layout 抽象成了一个 `LayoutManager`，`RecylerView` 不负责子 View 的布局， 我们可以自定义 `LayoutManager` 来实现不同的布局效果， 目前只提供了 `LinearLayoutManager`。 `LinearLayoutManager` 可以指定方向，默认是垂直， 可以指定水平， 这样就轻松实现了水平的 ListView。  

接下来看 `Adapter` 是怎么实现的

{% highlight java %}
public class MyAdapter extends RecyclerView.Adapter<MyAdapter.ViewHolder>{

    // 数据集
    private String[] mDataset;

    public MyAdapter(String[] dataset) {
        super();
        mDataset = dataset;
    }

    @Override
    public ViewHolder onCreateViewHolder(ViewGroup viewGroup, int i) {
        // 创建一个View，简单起见直接使用系统提供的布局，就是一个TextView
        View view = View.inflate(viewGroup.getContext(), android.R.layout.simple_list_item_1, null);
        // 创建一个ViewHolder
        ViewHolder holder = new ViewHolder(view);
        return holder;
    }

    @Override
    public void onBindViewHolder(ViewHolder viewHolder, int i) {
        // 绑定数据到ViewHolder上
        viewHolder.mTextView.setText(mDataset[i]);
    }

    @Override
    public int getItemCount() {
        return mDataset.length;
    }

    public static class ViewHolder extends RecyclerView.ViewHolder{

        public TextView mTextView;

        public ViewHolder(View itemView) {
            super(itemView);
            mTextView = (TextView) itemView;
        }
    }
}

{% endhighlight %}

`RecyclerView` 的另一个特点是标准化了 `ViewHolder`， 编写 `Adapter` 面向的是 `ViewHoder` 而不在是 `View` 了， 复用的逻辑被封装了， 写起来更加简单。  

###效果

<p>
   <img src="https://raw.githubusercontent.com/baoyongzhang/RecyclerViewDemo/master/screenshot.png" width="320" alt="Screenshot"/>
</p>


###总结
`RecyclerView` 简单使用之后， 发现确实灵活了很多， `RecyclerView` 的特性还有很多， 下一篇文章再继续探索，待续。。。

* [Demo 下载](https://github.com/baoyongzhang/RecyclerViewDemo)
