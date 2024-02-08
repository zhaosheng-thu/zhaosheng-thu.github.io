---
title: "Q&A4Android Development"
permalink: /knowledge/Android
date: 2024-02-09
excerpt: 'Some common questions and answers for Android development job interviews.'
---

# android 问题
一部分图片和内容参考了这篇文章[CSDN进程线程](https://blog.csdn.net/mu_wind/article/details/124616643?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522169795068916800225575693%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=169795068916800225575693&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-124616643-null-null.142^v96^pc_search_result_base1&utm_term=%E8%BF%9B%E7%A8%8B%E5%92%8C%E7%BA%BF%E7%A8%8B%E7%9A%84%E5%8C%BA%E5%88%AB&spm=1018.2226.3001.4187)
****
## 进程/线程区别
**定义:**

- **进程 (Process):** 进程是操作系统中的一个独立执行单元，每个进程都有自己独立的内存空间、系统资源（如文件描述符、网络连接等）以及代码执行路径。进程之间通常是相互独立的。

- **线程 (Thread):** 线程是在进程内部运行的较小的执行单元，共享进程的内存空间和资源。一个进程可以包含多个线程，这些线程可以并发执行。

**资源占用:**

- **进程:** 由于每个进程都有独立的内存空间和资源，创建、切换和销毁进程的开销较大。进程之间通常需要通过进程间通信（IPC）来共享数据。

- **线程:** 由于线程共享相同的内存空间和资源，创建、切换和销毁线程的开销较小。线程之间可以更容易地共享数据，但也需要注意同步问题，以避免竞态条件。

**并发性:**

- **进程:** 进程之间的并发性较低，因为它们是相互独立的，通常需要较复杂的机制来实现进程间的协同工作。

- **线程:** 线程之间的并发性较高，因为它们共享相同的资源。多线程可以更容易地实现并发操作，例如多线程编程可以提高多核处理器的利用率。

**安全性:**

- **进程:** 由于进程之间有独立的内存空间，一个进程的崩溃通常不会影响其他进程。因此，进程在安全性上有一定的优势。

- **线程:** 线程之间共享相同的内存空间，因此一个线程的错误可能会影响整个进程，导致进程崩溃。

**创建和销毁:**

- **进程:** 创建和销毁进程相对较慢，需要较多的系统资源和时间。

- **线程:** 创建和销毁线程相对较快，开销较小。

**通信和同步:**

- **进程:** 进程之间的通信通常需要使用IPC机制，如管道、消息队列、共享内存等。

- **线程:** 线程之间的通信和同步较为容易，可以使用线程间**共享内存**进行数据交换，同时也可以使用同步机制，如**互斥锁、信号量**等。

![线程的共享内存](https://zhaosheng-thu.github.io/files/gitfile/Thread-diff.jpg)
****

## Android应用程序的四大组件
是指构成Android应用的四个核心组件，它们用于实现不同的应用行为和功能。这些四大组件包括：

* Activity（活动）： Activity是用户界面的基本组成单元，用于展示用户交互的界面。每个屏幕或用户界面元素通常对应一个Activity。例如，一个登录界面、设置界面、地图界面等都可以是不同的Activity。Activity负责处理用户输入、界面交互和生命周期管理。

* Service（服务）： Service是一个后台组件，用于执行长时间运行的操作或在后台执行任务，而不需要与用户界面直接交互。例如，音乐播放、数据同步、定时提醒等都可以使用Service来实现。Service没有用户界面，通常在后台默默地运行。

* Broadcast Receiver（广播接收器）： 广播接收器用于接收系统广播消息或应用内部广播消息。它可以监听和响应特定的广播事件，无论应用是否处于活动状态。例如，接收系统的网络状态变化通知或接收自定义广播消息。

* Content Provider（内容提供器）： 内容提供器用于管理应用内部数据，以便其他应用可以访问和共享这些数据。它提供了一种标准的接口来对数据进行增删改查操作。内容提供器通常与数据库或文件存储结合使用，以提供数据访问的抽象层。

****


## 关于Activity组件

#### Activity启动流程
1. 用户点击应用图标：当用户在Launcher程序中点击应用图标时，触发了应用的启动请求。

2. ActivityManagerService介入：这个启动请求被传递给Android系统的`ActivityManagerService`（AMS），AMS负责管理所有应用和活动。

3. 检查应用是否已启动：AMS首先检查应用是否已经启动。如果应用尚未启动，AMS需要将其启动。

4. Zygote进程：如果应用尚未启动，AMS通知Zygote进程，这是一个专门用于孵化Android框架层和应用层进程的进程。Zygote是一个轻量级的进程，它可以快速孵化新的应用进程。

5. 应用进程孵化：Zygote进程接收到AMS的请求后，创建了一个新的应用进程，该进程是一个独立的Dalvik虚拟机实例。

6. ActivityThread的main方法：新的应用进程里执行ActivityThread的`main`方法，这是应用的主线程，负责应用的生命周期和事件处理。

7. 应用进程注册到AMS：应用进程启动后，它会通知AMS应用进程已经启动，并提供一个代理对象，AMS将使用这个代理对象与应用进程进行通信。

8. 创建入口Activity：AMS通知应用进程创建应用的入口Activity的实例，并执行其生命周期方法，如`onCreate()`.
#### Activity形态
* Active/Running:
Activity处于活动状态，此时Activity处于栈顶，是可见状态，可与用户进行交互。

* Paused：
当Activity失去焦点时，或被一个新的非全屏的Activity，或被一个透明的Activity放置在栈顶时，Activity就转化为Paused状态。但我们需要明白，此时Activity只是失去了与用户交互的能力，其所有的状态信息及其成员变量都还存在，只有在系统内存紧张的情况下，才有可能被系统回收掉。

* Stopped：
当一个Activity被另一个Activity完全覆盖时，被覆盖的Activity就会进入Stopped状态，此时它不再可见，但是跟Paused状态一样保持着其所有状态信息及其成员变量。

* Killed：
当Activity被系统回收掉时，Activity就处于Killed状态
#### Activity生命周期
![Activity](https://zhaosheng-thu.github.io/files/gitfile/activity-life.jpg)
##### 小结
- 当Activity启动时，依次调用onCreate(),onStart(),onResume()，
- 而当Activity退居后台时（不可见，点击Home或者被新的Activity完全覆盖），onPause()和onStop()会依次被调用。
- 当Activity重新回到前台（从桌面回到原Activity或者被覆盖后又回到原Activity）时，onRestart()，onStart()，onResume()会依次被调用。
- 当Activity退出销毁时（点击back键），onPause()，onStop()，onDestroy()会依次被调用，到此Activity的整个生命周期方法回调完成。

***

## 关于Service组件
- **Service执行线程**: Service在主线程（main Thread）中执行，因此不应执行耗时操作，如网络请求、数据库拷贝或处理大文件。这是为了确保应用的响应性。

- **Service进程设置**: 可以通过设置Service所在的进程，让Service在另一个进程中执行。这可以通过在Service组件的声明中设置`android:process`属性来实现。

- **执行时间限制**: Service执行的操作应受到时间限制。通常，Service的执行时间限制为最多20秒，BroadcastReceiver为10秒，Activity为5秒。这是为了有效分配系统资源。

- **Activity与Service交互**: Activity可以通过两种方式与Service进行交互。使用`bindService`方法来绑定Service，这允许Activity与Service进行通信。另一种方式是使用`startService`方法来启动Service，以执行后台任务。

- **IntentService**: IntentService是一个抽象类，继承自Service。它内部包含一个工作线程（HandlerThread）和一个处理器（Handler），用于处理异步请求。IntentService的特点是可以多次启动，每次请求都会以队列的方式在工作线程中执行，而且在任务执行完成后会自动停止Service。这使得它特别适合处理异步任务，而无需手动管理Service的生命周期。

#### Service生命周期
![线程的共享内存](https://zhaosheng-thu.github.io/files/gitfile/service-life.jpg)
***

## Fragment
Fragment是Android3.0开始新增的概念，意为碎片、片段。fragment是依赖于Activity的，不能独立存在的
#### fragment创建
- 静态创建：
1、创建XML布局文件
2、创建与Fragment对应的Java类：你需要创建一个与上述XML布局文件中指定的Fragment类名相匹配的Java类，这个类必须扩展自Fragment或其子类
3、在Activity的XML布局文件中配置参数
- 动态创建:
1、创建待添加的Fragment实例
2、获取FragmentManager
3、开启事务并使用replace()方法将Fragment添加到容器中
#### fragment生命周期
![fragment](https://zhaosheng-thu.github.io/files/gitfile/fragment-life.png)
#### fragment与activity通信
**Activity向Fragment**传值：
使用`setArguments(Bundle)`方法：
在Activity中创建一个Bundle对象，将要传递的值放入Bundle中，然后使用`setArguments(Bundle)`方法将Bundle对象传递给Fragment.

```java
Bundle bundle = new Bundle();
bundle.putString("key", "value");
YourFragment fragment = new YourFragment();
fragment.setArguments(bundle);

// 使用FragmentManager添加Fragment到Activity
FragmentManager fragmentManager = getSupportFragmentManager();
FragmentTransaction transaction = fragmentManager.beginTransaction();
transaction.add(R.id.fragment_container, fragment);
transaction.commit();
```
**Fragment向Activity**传值：
使用接口回调：
在Fragment中定义一个接口，其中包含一个方法，该方法用于传递值。然后，让Activity实现这个接口，使Activity必须实现这个方法。在Fragment中，调用接口方法来将值传递给Activity。
在Fragment中定义接口：

```java
public class YourFragment extends Fragment {
    public interface OnDataPass {
        void onDataPass(String data);
    }

    private OnDataPass dataPassListener;

    // 在需要传值的地方调用该方法
    public void sendDataToActivity(String data) {
        if (dataPassListener != null) {
            dataPassListener.onDataPass(data);
        }
    }
}

// 在Activity中实现接口：
public class YourActivity extends AppCompatActivity implements YourFragment.OnDataPass {
    @Override
    public void onDataPass(String data) {
        // 在这里处理从Fragment传递过来的值
    }
}

// 在Fragment中调用接口方法：
sendDataToActivity("Hello, Activity!")
```
***
## View相关
![fragment](https://zhaosheng-thu.github.io/files/gitfile/view-outline.jpg)
DecorView是顶级View，本质是一个FrameLayout它包含两部分，标题栏和内容栏，都是FrameLayout。内容栏id是content，也就是activity中设置setContentView的部分，最终将布局添加到id为content的FrameLayout中
#### View的绘制
1、**match_parent**（也称为fill_parent）：
当视图使用match_parent属性时，它会尽量占据其父容器的全部可用空间，以适应父容器的尺寸。如果一个视图的宽度或高度设置为match_parent，它将扩展到其父容器的宽度或高度，填充父容器的全部可用空间。这通常用于充满整个屏幕或充满父容器的情况，以充分利用可用空间。
2、**wrap_content**：
当视图使用wrap_content属性时，它将根据其内容的实际尺寸来确定所需的宽度和高度，以适应其内容。视图的尺寸将根据其中的文本、图像或其他内容自动调整，以便恰好包含其内容，不多不少。
3、**固定尺寸**
#### 自定义Viewgroup
自定义ViewGroup是在Android应用中创建自定义布局的一种方法，它允许您控制子视图的布局和排列方式。以下是创建自定义ViewGroup的一般步骤：
1 **创建一个自定义ViewGroup类**：
   首先，创建一个新的Java类，并让它继承自`ViewGroup`或其子类`RelativeLayout`、`LinearLayout`等，例如：

```java
    public class MyCustomViewGroup extends ViewGroup {
        public MyCustomViewGroup(Context context) {
            super(context);
        }

        public MyCustomViewGroup(Context context, AttributeSet attrs) {
            super(context, attrs);
        }

        // 可以添加自定义属性处理代码
    }
````
2 **实现onMeasure方法**：
在自定义ViewGroup的onMeasure方法中，需要遍历子视图并使用measureChild或measureChildWithMargins方法来测量它们。
````
@Override
protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
    // 遍历子视图并测量它们
    for (int i = 0; i < getChildCount(); i++) {
        View child = getChildAt(i);
        measureChild(child, widthMeasureSpec, heightMeasureSpec);
    }
    // 设置自身的测量尺寸
    setMeasuredDimension(measuredWidth, measuredHeight);
}
````
3 **实现onLayout方法**
在onLayout方法中排列子视图，指定它们的位置。计算每个子视图的左、上、右和底部边界，并使用layout方法来放置它们。
````
@Override
protected void onLayout(boolean changed, int left, int top, int right, int bottom) {
    int childLeft = getPaddingLeft();
    int childTop = getPaddingTop();
    for (int i = 0; i < getChildCount(); i++) {
        View child = getChildAt(i);
        // 计算子视图的位置
        child.layout(childLeft, childTop, childLeft + child.getMeasuredWidth(), childTop + child.getMeasuredHeight());
        childTop += child.getMeasuredHeight();
    }
}
````
4 在布局文件中使用自定义ViewGroup，并配置其属性以及添加子视图。

5 在Activity中使用自定义ViewGroup，将其添加到布局中。
***

## 类加载器
在 Android 开发中使用类加载器（ClassLoader）来实现插件化和组件化的技术

- Android 应用使用类加载器来加载和运行 Java 类。在 Android 平台上，虚拟机运行的是 Dex 字节码，而不是传统的 Java 类。

- Android 应用中的类文件通常被合并和优化，最终生成一个名为 `classes.dex` 的文件。这样做的目的是减少重复的类定义，以**减小 APK 文件**的大小。

- Android 中常用的类加载器有两种：DexClassLoader 和 PathClassLoader，它们都继承自 BaseDexClassLoader。

- DexClassLoader 和 PathClassLoader 的区别在于构造函数的参数。DexClassLoader 构造函数多了一个名为 `optimizedDirectory` 的参数，用于指定内部存储路径，用于缓存系统创建的 Dex 文件。而 PathClassLoader 的这个参数为 null，只能加载内部存储目录中的 Dex 文件。

- DexClassLoader 通常用于加载外部的 APK 文件，这是实现插件化技术的基础之一。通过 DexClassLoader，开发者可以在运行时加载和卸载插件。

总之，Android 的类加载器是实现插件化和组件化等高级应用结构的重要组成部分，允许动态加载和管理不同的类和模块。这种机制为 Android 应用的灵活性和可扩展性提供了支持。

