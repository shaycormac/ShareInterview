######1、画出Activity的生命周期图
![忘记是从哪篇文章里扒的图了](http://upload-images.jianshu.io/upload_images/1479978-a672e55cc06a5585.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

######2、介绍下不同场景下Activity生命周期的变化过程 
* 启动Activity： onCreate()--->onStart()--->onResume()，Activity进入运行状态。
* Activity退居后台： 当前Activity转到新的Activity界面或按Home键回到主屏： onPause()--->onStop()，进入停滞状态；这里有一种特殊情况，如果新Activity采用了透明主题，那么当前Activity不会回调onStop。
* Activity返回前台： onRestart()--->onStart()--->onResume()，再次回到运行状态。
* Activity退居后台，且系统内存不足， 系统会杀死这个后台状态的Activity，若再次回到这个Activity,则会走onCreate()-->onStart()--->onResume()
* 锁定屏与解锁屏幕 只会调用onPause()，而不会调用onStop方法，开屏后则调用onResume()

######3、当Activity A启动Activity B 时，生命周期执行过程？
A.onPause() --> 
B.onCreate() ,onStart(), onResume() --> 
A.onStop() 如果B 是个透明的,或者 是对话框的样式, 就不会调用 A.onStop()

######4、Activity跳转前要向SQLite里保存数据，应该在Activity中的哪个生命周期方法中执行？
* 根据生命周期或者第3题可知，Activity的onPause()方法更容易被触发，所以在这个方法中更好。
* 因为A.onPause() 先于B.onCreate()执行，如果在A.onPause()中做耗时操作会导致B不能更快的显示出来；所以要考虑好使用场景，做好取舍评估。

######5、内存不足时系统会杀掉后台的Activity，若需要进行一些临时状态的保存，在哪个方法进行？怎么恢复数据？
* Activity的 onSaveInstanceState() 和onRestoreInstanceState()并不是生命周期方法，它们不同于 onCreate()、onPause()等生命周期方法，它们并不一定会被触发。
* 当应用遇到意外情况（如：内存不足、用户直接按Home键）由系统销毁一个Activity，onSaveInstanceState() 会被调用。但是当用户主动去销毁一个Activity时，例如在应用中按返回键，onSaveInstanceState()就不会被调用。除非该activity是被用户主动销毁的，通常onSaveInstanceState()只适合用于保存一些临时性的状态，而onPause()适合用于数据的持久化保存。
* 重写onSaveInstanceState()方法，在此方法中保存需要保存的数据，该方法将会在activity被回收之前调用。通过重写onRestoreInstanceState()方法可以从中提取保存好的数据

######6、onSaveInstanceState()被执行的场景有哪些：
系统不知道你按下HOME后要运行多少其他的程序，自然也不知道activity A是否会被销毁，因此系统都会调用onSaveInstanceState()，让用户有机会保存某些非永久性的数据。以下几种情况的分析都遵循该原则
* 当用户按下HOME键时
* 长按HOME键，选择运行其他的程序时
* 锁屏时
* 从activity A中启动一个新的activity时
* 屏幕方向切换时

######7、Activity被回收数据保存和恢复代码
```
public class AllAppActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        //判断是否有以前的保存状态信息
        if(savedInstanceState!=null){ 
            savedInstanceState.get("key");
        }
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
    @Override
    protected void onSaveInstanceState(Bundle outState) {
        //可能被回收内存前保存状态和信息，
        Bundle data = new Bundle();
        data.putString("key", "value");
        outState.putAll(data);
        super.onSaveInstanceState(outState);
    }
    @Override
    protected void onRestoreInstanceState(Bundle savedInstanceState) {
        //判断是否有以前的保存状态信息
        if(savedInstanceState!=null){ 
            savedInstanceState.get("key");
        }
        super.onRestoreInstanceState(savedInstanceState);
    }
}
```

######8、如果Activity A的启动模式是SingleTask，跳转到Activity B 且返回值传给A ，要怎么处理？   *  Activity A使用了SingleTask模式在执行界面跳转的时候，多次启动此Activity都不会被重新创建，所以可能不会接收到传过来的Bundle里面的值，这样就导致传统的方法是接受不到返回值的。*  singleTask模式下，系统会回调onNewIntent()方法，在这个方法中可以调用 setIntent(intent);  这样就可以拿到Activity B跳到Activity A使用的Intent，从而拿到返回数据

######9、Activity的启动模式？
* standard：标准模式；这也是系统默认的模式。每次启动一个Activity都会重新创建一个新的实例，不管这个实例是否存在。
* singleTop:栈顶复用模式；在这种模式下,如果Activity已经位于任务栈的栈顶，那么此Activity不会被创建，同时他的onNewIntent方法会被调用。如果Activity的实例已经存在但不是位于栈顶，那么新的Activity依然会被重新创建。
* singleTask：栈内复用模式，在这种模式下，如果activty在一个栈中存在，那么多次启动此Activity都不会被重新创建，系统也会回调onNewIntent
* singleInstace：单实例模式，这是一种加强的singleTask模式，他除了具有singleTask模式的特性外，还加强了一点，那就是具有此模式的Activity只能单独的位于一个任务栈中。

######10、如何给Activity指定启动模式？
* 在AndroidMenifest.xml中指定
    ```
<activity
android:name="com.ryg.chapter_1.SecondActivity"
android:configChanges="screenLayout"
android:label="@string/app_name"
android:launchMode="standard"
android:taskAffinity="com.ryg.task1" />
    ```
* 通过在Intent中设置标志位来指定启动模式
```
Intent intent = new Intent();
intent.setClass(A_Activity.this,B_Activity.class);
intent.addFlags(Intenet.FLAGS_ACTIVITY_NEW_TASK);
startIntent(intent);
```
* 俩种指定启动模式的区别：
  * 第二种方式的优先级高于第一种；
  * 俩种方式在限定范围上有所不同：比如，
    第一种方式无法为Activity设定FLAG_ACTIVITY_CLEAR_TOP标识，
    第二种方式无法为Activity指定singleInstace模式。

######11、 横竖屏切换时候activity的生命周期? 
* 不设置Activity的`android:configChanges`时，切屏会重新调用各个生命周期，切横屏时会执行一次，切竖屏时会执行两次 
* 设置Activity的`android:configChanges="orientation"`时，切屏还是会重新调用各个生命周期，切横、竖屏时只会执行一次 
* 设置Activity的`android:configChanges="orientation|keyboardHidden"`时，切屏不会重新调用各个生命周期，只会执行onConfigurationChanged方法 

######12、如何将一个 Activity 设置成窗口的样式？ 
只需要给我们的 Activity 配置如下属性即可`android:theme="@android:style/Theme.Dialog"` 

######13、你知道onNewIntent吗？ 
如果IntentActivity处于任务栈的顶端，也就是说之前打开过的Activity，现在处于onPause、onStop 状态的话，其他应用再发送Intent的话，执行顺序为： onNewIntent，onRestart，onStart，onResume。

######14、如何获取当前屏幕Activity的对象？ 
使用ActivityLifecycleCallbacks 
 
######15、如何判断一个Activity是否在运行？
```
if (activity == null || activity.isDestroyed() || activity.isFinishing()) {
    YES
}
```

######16、同一个程序，但不同的Activity是否可以放在不同的Task任务栈中？
可以放在不同的Task中。需要为不同的activity设置不同的affinity属性，启动activity的Intent需要包含FLAG_ACTIVITY_NEW_TASK标记。

######17、Activity中 this、getApplicationContext和getActivity的区别
* this:代表当前,在Activity当中就是代表当前的Activity，换句话说就是 Activity.this在Activity当中可以缩写为this.
* getActivity()指的是在fragment当中调用得到他所在的Activity
* getApplicationContext():生命周期是整个应用，应用摧毁，它才摧毁。

######18、Application（或者Service）和Activity都可以调用Context的startActivity方法，那么在这两个地方调用startActivity有区别吗？
在Application（或者Service）需要给Intent设置Intent.FLAG_ACTIVITY_NEW_TASK才能正常启动Activity

######19、Activity之间的数据传递有哪些方式？
* intent.putExtra 方法
* 使用全局变量Application
* 使用静态变量
*  剪切板ClipboardManager传递数据

######20、Fragment 的好处：
* Fragment 可以使你能够将 activity 分离成多个可重用的组件，每个都有它自己的生命周期和UI。
* Fragment 可以轻松得创建动态灵活的 UI 设计，可以适应于不同的屏幕尺寸。从手机到平板电脑。
* Fragment 是一个独立的模块,紧紧地与 activity 绑定在一起。可以运行中动态地移除、加入、交换等。
* Fragment 提供一个新的方式让你在不同的安卓设备上统一你的 UI。
* Fragment 解决 Activity 间的切换不流畅，轻量切换。
* Fragment 替代 TabActivity 做导航，性能更好。
* Fragment 在 4.2.版本中新增嵌套 fragment 使用方法，能够生成更好的界面效果

######21、Fragment 在你们项目中的使用
Fragment 是 android3.0 以后引入的的概念，做局部内容更新更方便，原来为了到达这一点要把多个布局放到一个 activity 里面，现在可以用多 Fragment 来代替，只有在需要的时候才加载Fragment，提高性能。

######22、Fragment 跟 Activity 之间是如何传值的
* 当 Fragment 跟 Activity 绑定之后，在 Fragment 中可以直接通过 getActivity（）方法获取到其绑定的 Activity 对象，这样就可以调用 Activity 的方法了。
* 在 Activity 中可以通过如下方法获取到Fragment 实例
`FragmentManager fragmentManager = getFragmentManager();
Fragment fragment = fragmentManager.findFragmentByTag(tag);
Fragment fragment = fragmentManager.findFragmentById(id);`
获取到 Fragment 之后就可以调用 Fragment 的方法。也就实现了通信功能

######23、Intent的原理，作用，可以传递哪些类型的参数？
* intent是连接Activity, Service, BroadcastReceiver, ContentProvider四大组件的信使,，可以传递八种基本数据类型以及string, Bundle类型，以及实现了Serializable或者Parcelable的类型。
* Intent可以划分成显式意图和隐式意图。 
  * 显式意图：调用Intent.setComponent()或Intent.setClass()方法明确指定了组件名的Intent为显式意图，显式意图明确指定了Intent应该传递给哪个组件。 
  * 隐式意图：没有明确指定组件名的Intent为隐式意图。 Android系统会根据隐式意图中设置的动作(action)、类别(category)、数据（URI和数据类型）找到最合适的组件来处理这个意图。

######24、service和Thread区别？
* Thread 是程序执行的最小单元，它是分配CPU的基本单位。可以用 Thread 来执行一些异步的操作。
* Service是一个专门在后台处理长时间任务的Android组件，它没有UI
* Service 是android的一种机制，当它运行的时候如果是Local Service，那么对应的 Service 是运行在主进程的 main 线程上的。如：onCreate，onStart 这些函数在被系统调用的时候都是在主进程的 main 线程上运行的。如果是Remote Service，那么对应的 Service 则是运行在独立进程的 main 线程上

######25、Service的启动方式及其区别？
Service有俩中启动方式，分别是startService和bindService。
* 绑定？
  * startService：只是启动Service，Activity和Service并没有绑定，只有当Service调用stopService服务才会终止。
  * bindService：这种启动方式Activity和Service进行了绑定，启动Service的组件可以通过回调获取Service的代理对象和Service交互；当启动方销毁时，Service也会自动进行unBind操作，当发现所有绑定都进行了unBind时才会销毁Service；
* 生命周期
  * startService：`context.startService() ->onCreate()- >onStartCommand()->Service running--调用context.stopService() ->onDestroy() `
  * bindService：`context.bindService()->onCreate()->onBind()->Service running--调用>onUnbind() -> onDestroy() `

######26、Activity 怎么和 Service 绑定,怎么在 Activity 中启动自 己对应的 Service? 
* Activity 通过 bindService(Intent service, ServiceConnection conn, int flags)跟 Service 进行绑定,当绑定成功的时候 Service 会将代理对象通过回调 的形式传给 conn,这样我们就拿到了 Service 提供的服务代理对象。 
* 在 Activity 中可以通过 startService 和 bindService 方法启动 Service。一 般情况下如果想获取 Service 的服务对象那么肯定需要通过 bindService()方 法,比如音乐播放器,第三方支付等。如果仅仅只是为了开启一个后台任务那么 可以使用 startService()方法。

######27、Service中可以弹Toast吗？
* 这个问题其实就是问一下Service是执行在UI线程中吗？类似的问题还有“Service的onCreate回调函数可以做耗时的操作吗？”，“Service 是否在 main thread 中执行”，“Service 和 Activity 在同一个线程吗？”等；
* 我们要牢记一句真理“默认情况下四大组件都是在UI线程中执行的”，Service 本身就是 Context 的子类，我们可以获取到Context对象，所以Service中当然可以弹Toast，同理，Service的onCreate回调函数不可以做耗时的操作；
* 特殊情况 ,可以在清单文件配置 service 执行所在的进程 ,让 service 在另外的进程中执行
`<service
android:name="com.baidu.location.f"
android:enabled="true"
android:process=":remote" >
</service>
`

######28、如何保证Service在后台不被kill
* 重写onStartCommand返回START_STICKY,START_STICKY是service被kill掉后自动重写创建
* 在onDestroy里启动service
* 监听系统广播，如果还能监听到系统广播就说明服务没有被Kill
* Application加上Persistent属性,让应用变成随系统关闭而关闭。
* 提升Service优先级:设置android:priority = “1000”
* 放一个像素在桌面
*  Android中的进程是托管的，当系统进程空间紧张的时候，会依照优先级自动进行进程的回收 当service运行在低内存的环境时，将会kill掉一些存在的进程。因此进程的优先级将会很重要，可以使用startForeground 将service放到前台状态。这样在低内存时被kill的几率会低一些。

######29、进程的优先级
* 前台进程
* 可视进程
* 服务进程
* 后台进程
* 空进程

######30、IntentService与Service的区别？
* Service 也不是专门一条新线程,因此不应该在 Service 中直接处理耗时的 任务; 
* Service 不会专门启动一条单独的进程,Service 与它所在应用位于同一个进 程中; 
* IntentService 是 Service 的子类，是一个异步的，会自动停止的服务，很好解决了传统的Service中处理完耗时操作忘记停止并销毁Service的问题
* IntentService 会创建独立的worker线程来处理所有的Intent请求；
* IntentService不会阻塞UI线程，而普通Serveice会导致ANR异常
* Intentservice若未执行完成上一次的任务，将不会新开一个线程，是等待之前的任务完成后，再执行新的任务，等任务完成后再次调用stopSelf()
* 正在运行的 IntentService 的程序相比起纯粹的后台程序更不容易被系统杀死，该程序的优先级是介于前台程序与纯后台程序之间的

######31、Service是否正在运行
```
    private boolean isServiceRunning() {
        ActivityManager manager = (ActivityManager) getSystemService(ACTIVITY_SERVICE);
        {
            if ("com.example.demo.BindService".equals(service.service.getClassName())) {
                return true;
            }
        }
        return false;
    }
```

######32、Android Service与Activity之间的通信方式？
* 通过Binder对象 
当Activity通过调用bindService(Intent service, ServiceConnection conn,int flags),得到一个Service的一个对象，通过这个对象我们可以直接访问Service中的方法。
  * 添加一个继承Binder的内部类，并添加相应的逻辑方法
  * 重写Service的onBind方法，返回我们刚刚定义的那个内部类实
  * Activity中创建一个ServiceConnection的匿名内部类，并且重写里面的onServiceConnected方法和onServiceDisconnected方法，这两个方法分别会在活动与服务成功绑定以及解除绑定的时候调用，在onServiceConnected方法中，我们可以得到一个刚才那个service的binder对象，通过对这个binder对象进行向下转型，得到我们那个自定义的Binder实例，有了这个实例，做可以调用这个实例里面的具体方法进行需要的操作了
* 通过Broadcast Receiver
当我们的进度发生变化的时候我们发送一条广播，然后在Activity的注册广播接收器，接收到广播之后更新视图
* EventBus

######33、[广播概述](http://www.jianshu.com/p/57a1899c17fb)

######34、Android引入广播机制的用意?
* 从MVC的角度考虑(应用程序内)
四大组件本质上就是为了实现移动或者说嵌入式设备上的MVC架构，它们之间有时候是一种相互依存的关系，有时候又是一种补充关系，引入广播机制可以方便几大组件的信息和数据交互。
* 程序间互通消息(例如在自己的应用程序内监听系统来电)
* 效率上(参考UDP的广播协议在局域网的方便性)
* 设计模式上(反转控制的一种应用，类似监听者模式)

######35、为什么要用 ContentProvider?它和 sql 的实现上有什么差别？
* ContentProvider 屏蔽了数据存储的细节,内部实现对用户完全透明,用户只 需要关心操作数据的 uri 就可以了,ContentProvider 可以实现不同 app 之间 共享。Sql只能在该工程的内部共享数据，ContentProvider能在工程之间实现数据共享。
* Sql 也有增删改查的方法,但是 sql 只能查询本应用下的数据库。而 ContentProvider 还可以去增删改查本地文件. xml 文件的读取等。

######36、ContentProvider的URI的配置？
清单文件之指定URI或者代码里面指定URI，contentProvider通过URI访问数据

######37、如何通过一套标准及统一的接口获取其他应用程序暴露的数据？
Android提供了ContentResolver，外界的程序可以通过ContentResolver接口访问ContentProvider提供的数据。

######38、contentprovider怎么实现数据共享？
一个程序可以通过实现一个Content provider的抽象接口将自己的数据完全暴露出去，而且Content providers是以类似数据库中表的方式将数据暴露。Content providers存储和检索数据，通过它可以让所有的应用程序访问到，这也是应用程序之间唯一共享数据的方法。要想使应用程序的数据公开化，可通过2种方法：创建一个属于你自己的Content provider或者将你的数据添加到一个已经存在的Content provider中，前提是有相同数据类型并且有写入Content provider的权限。

######39、Android如何访问自定义ContentProvider
第一：得到ContentResolver类对象：ContentResolver cr = getContentResolver（）；
第二：定义要查询的字段String数组。
第三：使用cr.query();返回一个Cursor对象。
第四：使用while循环得到Cursor里面的内容

######40、Android中Activity, Intent, Content Provider, Service各有什么区别。
* Activity： 活动，是最基本的android应用程序组件。一个活动就是一个单独的屏幕，每一个活动都被实现为一个独立的类，并且从活动基类继承而来。
* Intent： 意图，描述应用想干什么。最重要的部分是动作和动作对应的数据。
* Content Provider：内容提供器，android应用程序能够将它们的数据保存到文件、SQLite数据库中，甚至是任何有效的设备中。当你想将你的应用数据和其他应用共享时，内容提供器就可以发挥作用了。
* Service：服务，具有一段较长生命周期且没有用户界面的程序。

![0.jpg](http://upload-images.jianshu.io/upload_images/1479978-0ff1a43230b41689.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
