####登录时怎么保存用户名密码实现下次自动登录
首先，我们要实现记住密码的功能,在Android中，轻量级的数据存储一般使用SharedPreferences，当然你也可以使用其他的数据存储方式。SharedPreferences实现记住密码功能一般遵循以下步骤：
1. 每次登录时，都检测上次登录是否记住了密码，如果记住了就要从sp里读取账户信息，并自动填写到输入框里
2. 登录成功后，要检测本次登录是否选择了记住密码，如果为true，则需要把本次登录的帐号信息写到sp里，如果为false，则需要清空sp的key-value

另外，这里只所以说记住密码，而没有说记住用户名和密码，我相信这个逻辑大家都可以理得通，没有用户名的密码是无意义的，记住密码的言外之意就是记住用户名和密码。

activity_login.xml:
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context="com.android.test.LoginActivity">
    <EditText
        android:id="@+id/username"
        android:layout_width="match_parent"
        android:layout_height="48dip"
        android:layout_marginTop="8dip"
        android:hint="输入用户名" />
    <EditText
        android:id="@+id/password"
        android:layout_width="match_parent"
        android:layout_height="48dip"
        android:layout_marginTop="8dip"
        android:hint="输入密码" />
    <CheckBox
        android:id="@+id/remembers_password"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="8dip"
        android:text="记住密码" />
    <Button
        android:id="@+id/login"
        android:layout_width="match_parent"
        android:layout_height="48dip"
        android:layout_marginTop="8dip"
        android:onClick="exeLogin"
        android:text="登录" />
</LinearLayout>
```

![QQ截图20160620143545.png](http://upload-images.jianshu.io/upload_images/1479978-2ba937438689fb79.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
public class LoginActivity extends AppCompatActivity {

    // 假设服务器里有这个账户信息
    private static final String U_name = "zhangsan";
    private static final String U_pswd = "zhang@123";

    private EditText username, password;
    private CheckBox remembers_password;
    private Button login;

    private SharedPreferences sp;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_login);

        username = (EditText) findViewById(R.id.username);
        password = (EditText) findViewById(R.id.password);
        remembers_password = (CheckBox) findViewById(R.id.remembers_password);
        login = (Button) findViewById(R.id.login);
        sp = PreferenceManager.getDefaultSharedPreferences(this);

        String name = sp.getString("USERNAME", "");
        String pass = sp.getString("PASSWORD", "");
        boolean rememberPassword = sp.getBoolean("REMEBERS_PASSWORD", false);


        //如果上次选了记住密码，那么本次登录会自动填写用户名和密码
        if (rememberPassword) {
            username.setText(name);
            password.setText(pass);
            remembers_password.setChecked(true);
        }

    }

    // 点击登录按钮
    public void exeLogin(View view) {
        String inputUsername = username.getText().toString();
        String inputUserpswd = password.getText().toString();

        if (inputUsername.equals(U_name) && inputUserpswd.equals(U_pswd)) {
            Toast.makeText(LoginActivity.this, "登录成功", Toast.LENGTH_LONG).show();

            //是否记住密码
            if(remembers_password.isChecked()){
                // 保存信息
                sp.edit().putBoolean("REMEBERS_PASSWORD", true).commit();
                sp.edit().putString("USERNAME", inputUsername).commit();
                sp.edit().putString("PASSWORD", inputUserpswd).commit();
            }else{
                // 注意，如果本次登录不记住密码的话，需要把sp里的信息清空,当然这里也可以直接删除sp里key-value
                sp.edit().putBoolean("REMEBERS_PASSWORD", false).commit();
                sp.edit().putString("USERNAME", "").commit();
                sp.edit().putString("PASSWORD", "").commit();
            }

            startActivity(new Intent(LoginActivity.this,LoginSucActivity.class));

        } else {
            Toast.makeText(LoginActivity.this, "登录失败", Toast.LENGTH_LONG).show();
        }
    }
}
```
以上，我们实现了记住密码功能，接下来就要在此基础上实现自动登录的功能，只需要：
```html
        if (rememberPassword) {
            username.setText(name);
            password.setText(pass);
            remembers_password.setChecked(true);
        }
在此处增加以下代码：
        if(remebersLoginSelf){
            loginself.setChecked(true);
            exeLogin(login);
        }
```
然后，
```
            //是否记住密码
            if (remembers_password.isChecked()) {
                // 保存信息
                sp.edit().putBoolean("REMEBERS_PASSWORD", true).commit();
                sp.edit().putString("USERNAME", inputUsername).commit();
                sp.edit().putString("PASSWORD", inputUserpswd).commit();
            } else {
                // 注意，如果本次登录不记住密码的话，需要把sp里的信息清空,当然这里也可以直接删除sp里key-value
                sp.edit().putBoolean("REMEBERS_PASSWORD", false).commit();
                sp.edit().putString("USERNAME", "").commit();
                sp.edit().putString("PASSWORD", "").commit();
            }
在此处增加以下代码：
            if (loginself.isChecked()){
                sp.edit().putBoolean("LOGIN_SELF", true).commit();
            }else{
                sp.edit().putBoolean("LOGIN_SELF", false).commit();
            }
```
以上代码在[ 百度云-自动登录](http://pan.baidu.com/s/1bXnBLc)

当然，这里只是个demo，更细致的逻辑还需要你自己去处理，比如：自动登录的前提就是记住密码等。

但是，利用sp虽然实现了我们自动登录的需求，但是实际上一般我们不会这么做，因为这样做存在一定的安全隐患，我们可以在sp中存取数据时进行加密解密，这样的话安全性会相对高一些。当然这里还有更为安全的一种做法：TOKEN思想，也就是说我们在登录的时候，服务器给我们返回一个token值，我们把这个值保存到起来，下次请求接口时，带上这个 token ，服务端判断这个token是否有效或过期，返回对应的错误码，如果本地不存在 token，就是没有登录过，跳转登陆。

####如果sp只存储用户名，比如三个用户都存在sp里，取出来怎么取？存进去怎么存？你怎么区分 ？
利用SharedPreferences存储数据，首先我们要获取一个 SharedPreferences实例，我通常一般会使用PreferenceManager.getDefaultSharedPreferences(this);来获取这个实例， 我们知道SharedPreferences有如下**操作模式：**
* mode指定为MODE_PRIVATE(或0)，则该配置文件只能被自己的应用程序访问。
* mode指定为MODE_WORLD_READABLE，则该配置文件除了自己访问外还可以被其它应该程序读取。
* mode指定为MODE_WORLD_WRITEABLE，则该配置文件除了自己访问外还可以被其它应该程序读取和写入

其实，除了以上获取SharedPreferences实例的方法外，目前android还气功了其他获取SharedPreferences实例的方法，细看源码就可以比较出他们的不同之处：
1. **sp = Context.getSharedPreferences("sp_name",0);**
得到一个名为sp_name的实例，sp_name为本组件的配置文件名( 自己定义，也就是一个文件名)，当这个文件不存在时，直接创建，如果已经存在，则直接使用，mode为操作模式，默认的模式为0或MODE_PRIVATE，
2. **sp = Activity.getPreferences(0);**
得到一个名为activity名的实例,其中配置文件仅可以被调用的Activity使用
3. **sp = PreferenceManager.getDefaultSharedPreferences(this);**
等到一个名为：yourpackageName_preferences的示例

发现什么了吗？回到我们的问题：“*如果sp只存储用户名，比如三个用户都存在sp里，取出来怎么取？存进去怎么存？你怎么区分 ？*”。第一个方法说“得到一个名为sp_name的实例”，也就是说我你们可以自定义SharedPreferences的文件名，更直白点说就是我们可以把不同组合的数据存储到不同的文件中去，这样即使有多个“user_name”的key，也不会影响我们的存取，因为他们虽然key值相同，但所处的文件不同，也就是此key非彼key。