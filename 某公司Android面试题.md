

#### 1、请写出 Object 类中的方法有哪些？
[java.lang.Object 请查看源码：](https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html)   
https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html

<table class="overviewSummary" border="0" cellpadding="3" cellspacing="0" summary="Method Summary table, listing methods, and an explanation">
<tr>
<th class="colFirst" scope="col">Modifier and Type</th>
<th class="colLast" scope="col">Method and Description</th>
</tr>
<tr class="altColor">
<td class="colFirst"><code>protected <a href="../../java/lang/Object.html" title="class in java.lang">Object</a></code></td>
<td class="colLast"><code><strong><a href="../../java/lang/Object.html#clone()">clone</a></strong>()</code>
<div class="block">Creates and returns a copy of this object.</div>
</td>
</tr>
<tr class="rowColor">
<td class="colFirst"><code>boolean</code></td>
<td class="colLast"><code><strong><a href="../../java/lang/Object.html#equals(java.lang.Object)">equals</a></strong>(<a href="../../java/lang/Object.html" title="class in java.lang">Object</a>&nbsp;obj)</code>
<div class="block">Indicates whether some other object is "equal to" this one.</div>
</td>
</tr>
<tr class="altColor">
<td class="colFirst"><code>protected void</code></td>
<td class="colLast"><code><strong><a href="../../java/lang/Object.html#finalize()">finalize</a></strong>()</code>
<div class="block">Called by the garbage collector on an object when garbage collection
 determines that there are no more references to the object.</div>
</td>
</tr>
<tr class="rowColor">
<td class="colFirst"><code><a href="../../java/lang/Class.html" title="class in java.lang">Class</a>&lt;?&gt;</code></td>
<td class="colLast"><code><strong><a href="../../java/lang/Object.html#getClass()">getClass</a></strong>()</code>
<div class="block">Returns the runtime class of this <code>Object</code>.</div>
</td>
</tr>
<tr class="altColor">
<td class="colFirst"><code>int</code></td>
<td class="colLast"><code><strong><a href="../../java/lang/Object.html#hashCode()">hashCode</a></strong>()</code>
<div class="block">Returns a hash code value for the object.</div>
</td>
</tr>
<tr class="rowColor">
<td class="colFirst"><code>void</code></td>
<td class="colLast"><code><strong><a href="../../java/lang/Object.html#notify()">notify</a></strong>()</code>
<div class="block">Wakes up a single thread that is waiting on this object's
 monitor.</div>
</td>
</tr>
<tr class="altColor">
<td class="colFirst"><code>void</code></td>
<td class="colLast"><code><strong><a href="../../java/lang/Object.html#notifyAll()">notifyAll</a></strong>()</code>
<div class="block">Wakes up all threads that are waiting on this object's monitor.</div>
</td>
</tr>
<tr class="rowColor">
<td class="colFirst"><code><a href="../../java/lang/String.html" title="class in java.lang">String</a></code></td>
<td class="colLast"><code><strong><a href="../../java/lang/Object.html#toString()">toString</a></strong>()</code>
<div class="block">Returns a string representation of the object.</div>
</td>
</tr>
<tr class="altColor">
<td class="colFirst"><code>void</code></td>
<td class="colLast"><code><strong><a href="../../java/lang/Object.html#wait()">wait</a></strong>()</code>
<div class="block">Causes the current thread to wait until another thread invokes the
 <a href="../../java/lang/Object.html#notify()"><code>notify()</code></a> method or the
 <a href="../../java/lang/Object.html#notifyAll()"><code>notifyAll()</code></a> method for this object.</div>
</td>
</tr>
<tr class="rowColor">
<td class="colFirst"><code>void</code></td>
<td class="colLast"><code><strong><a href="../../java/lang/Object.html#wait(long)">wait</a></strong>(long&nbsp;timeout)</code>
<div class="block">Causes the current thread to wait until either another thread invokes the
 <a href="../../java/lang/Object.html#notify()"><code>notify()</code></a> method or the
 <a href="../../java/lang/Object.html#notifyAll()"><code>notifyAll()</code></a> method for this object, or a
 specified amount of time has elapsed.</div>
</td>
</tr>
<tr class="altColor">
<td class="colFirst"><code>void</code></td>
<td class="colLast"><code><strong><a href="../../java/lang/Object.html#wait(long,%20int)">wait</a></strong>(long&nbsp;timeout,
    int&nbsp;nanos)</code>
<div class="block">Causes the current thread to wait until another thread invokes the
 <a href="../../java/lang/Object.html#notify()"><code>notify()</code></a> method or the
 <a href="../../java/lang/Object.html#notifyAll()"><code>notifyAll()</code></a> method for this object, or
 some other thread interrupts the current thread, or a certain
 amount of real time has elapsed.</div>
</td>
</tr>
</table>




#### 2、请写出屏幕适配中 dp 和 px 的转换公式？
	    public static int dp2px(int dp) {
	        return (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP, dp, context.getResources().getDisplayMetrics());
	    }
	
	    public static int px2dp(float pxValue) {
	        return (int) (pxValue / context.getResources().getDisplayMetrics().density + 0.5f);
	    }
[Android中 dp、px、sp的相互转换](http://www.jianshu.com/p/103b3c066606)   
[面试 | Android View、布局、动画、适配](http://www.jianshu.com/p/0f01273d958a)

#### 3、SharedPreferences 保存文件的路径和扩展名？
/data/data/<package name>/shared_prefs

#### 5、用 SimpleAdapter 作为 ListView的适配器，支持的 View 类型有哪几种？
TextView，ImageView， 看源码：

	private void bindView(int position, View view) {
        final Map dataSet = mData.get(position);
        if (dataSet == null) {
            return;
        }

        final ViewBinder binder = mViewBinder;
        final String[] from = mFrom;
        final int[] to = mTo;
        final int count = to.length;

        for (int i = 0; i < count; i++) {
            final View v = view.findViewById(to[i]);
            if (v != null) {
                final Object data = dataSet.get(from[i]);
                String text = data == null ? "" : data.toString();
                if (text == null) {
                    text = "";
                }

                boolean bound = false;
                if (binder != null) {
                    bound = binder.setViewValue(v, data, text);
                }

                if (!bound) {
                    if (v instanceof Checkable) {
                        if (data instanceof Boolean) {
                            ((Checkable) v).setChecked((Boolean) data);
                        } else if (v instanceof TextView) {
                            // Note: keep the instanceof TextView check at the bottom of these
                            // ifs since a lot of views are TextViews (e.g. CheckBoxes).
                            setViewText((TextView) v, text);
                        } else {
                            throw new IllegalStateException(v.getClass().getName() +
                                    " should be bound to a Boolean, not a " +
                                    (data == null ? "<unknown type>" : data.getClass()));
                        }
                    } else if (v instanceof TextView) {
                        // Note: keep the instanceof TextView check at the bottom of these
                        // ifs since a lot of views are TextViews (e.g. CheckBoxes).
                        setViewText((TextView) v, text);
                    } else if (v instanceof ImageView) {
                        if (data instanceof Integer) {
                            setViewImage((ImageView) v, (Integer) data);                            
                        } else {
                            setViewImage((ImageView) v, text);
                        }
                    } else {
                        throw new IllegalStateException(v.getClass().getName() + " is not a " +
                                " view that can be bounds by this SimpleAdapter");
                    }
                }
            }
        }
    }

#### 6、Android中有哪几种进程？
[面试 | Android四大组件 更新篇 第29](https://github.com/jasonLYF/ShareInterview/blob/master/%E9%9D%A2%E8%AF%95-Android%E5%9B%9B%E5%A4%A7%E7%BB%84%E4%BB%B6.md)

#### 7、如何修改以下代码将其正确？
        ArrayList list = new ArrayList();
        String str = "0";
        list.add(str);
        list.add(new Integer(1));
        list.add(new Double(2));
        System.out.println((String) list.get(1));

先来运行下源代码看看会报什么异常？
![2017-03-20_202027.png](http://upload-images.jianshu.io/upload_images/1479978-9d6cb58933479eba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上文中的24行对应的就是` System.out.println((String) list.get(1));`这行代码；   

改法一：` System.out.println(list.get(1).toString());`   
改法二：`list.add(new Integer(1).toString()); list.add(new Double(2).toString());`

#### 8、下面程序是否存在问题？为什么？怎么解决？
        new Thread() {
            Handler handler = null;
            public void run() {
                handler = new Handler();  // line 23
            }
        }.start();
把他放到开发工具里看看会有什么异常？
![](http://upload-images.jianshu.io/upload_images/1479978-fc051f877809ab9a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在 Android 的 UI 线程中创建Handler时，会自动调用 Looper.prepare（）方法去给当前线程设置一个Looper对象去创建一个消息队列，但是在子线程中不会自动调用这个方法，也就是说在非主线程中默认是没有创建 Looper 对象，所以我们再子线程中创建 Handler 的时候要去手动调用一下这个方法，这样的话就可以让消息处理在该线程中完成。所以代码可以修改为如下：

        new Thread() {
            Handler handler = null;
            public void run() {
                Looper.prepare();
                handler = new Handler();
                Looper.loop();
            }
        }.start();
`Looper.loop()`表示让Looper开始工作，从消息队列里取消息，处理消息。    
当然，这种方法只是让子线程可以处理消息队列，但还是不能刷新 UI ，因为 Android 是单线程模型的只能在 UI 线程中去更新 UI ;

如果想要让子线程也可以更新 UI 我们可以改为如下的样子：

        new Thread() {
            Handler handler = null;
            public void run() {
                handler = new Handler(Looper.getMainLooper());
            }
        }.start();
`Looper.getMainLooper()`这个方法获取的是主线程的Looper对象，表示放到主UI线程去处理；

#### 9、Http中get和post的区别？
[面试-Java基础](https://github.com/jasonLYF/ShareInterview/blob/master/%E9%9D%A2%E8%AF%95-Java%E5%9F%BA%E7%A1%80.md)

#### [10、用于监听官博事件的俩中注册方法，有什么区别？](http://www.jianshu.com/p/57a1899c17fb)

#### 11、清除缓存 和 清理数据 的区别？
清除缓存：   
缓存是应用在运行过程中产生的一些临时数据，清理后应用再次运行的时候会感觉“稍慢”，那是因为它重新从网络拉取数据，清理缓存后手机的运行内存增大；   
清理数据：   
对用户来说就是清除一些用户信息，比如登录信息、进度保存信息等，清理之后，再次运行对应的App，比如微信，则需要重新输入登录信息；而对于开发者来说就是清理一些配置信息，比如SharedPreferences、数据库等；

#### 12、什么是ANR?怎么避免？
Application Not Responding，即应用无响应；   
造成的原因有如下几种：   
1>事件5s内没有响应；2>广播事件10s内没有处理完成；3>service在20秒内没有处理完成；   
解决办法：   
1> 耗时操作如网络或数据库操作等交给子线程来处理，主线程只处理UI   
2> 用Handler实现主线程和子线程之间的交互；   
3> 避免在BroadcastReceiver里做耗时的操作或计算   
[android优化篇四性能优化总结](https://github.com/jasonLYF/jason-blog/blob/master/android%E4%BC%98%E5%8C%96%E7%AF%87%E5%9B%9B%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96%E6%80%BB%E7%BB%93.md)

#### [13、事件分发机制](http://www.jianshu.com/p/86e7cd8bc73f)

#### 14、将字符串"This is Android"传换成"AndroidisThis",不能用split；
我不想写了，你们来写吧：）


![](http://upload-images.jianshu.io/upload_images/1479978-0ff1a43230b41689.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
