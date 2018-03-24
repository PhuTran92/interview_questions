There are four different types of app components:
Activities: Là màn hình một màn hình giao diện ứng dụng mà người dùng có thể thao tác dc trên đó. Tại 1 thời điểm chỉ có 1 activity đc chạy 

Services: Là 1 component thường dc sử dụng để chạy ứng dụng dưới background, thực hiện các long-task. Server không có UI. Lưu ý: service chạy trên main thread chứ ko phải background 
	
	// TODO: phân biệt giữa intent service và server

	Server: run on main thread, have to call stopService()
	Intent service: support run task on background, auto destroy when done. one at a time. This is the best option if you don't require that your service handle multiple requests simultaneously

	// TODO: phân biệt giữa bind service và started service

	Started service: is one that another component starts by calling startService(), the service should stop itself when its job is complete by calling stopSelf(), or another component can stop it by calling stopService().

	A bound service: is one that allows application components to bind to it by calling bindService() to create a long-standing connection. It generally doesn't allow components to start it by calling startService().


	A foreground service is a service that the user is actively aware of and is not a candidate for the system to kill when low on memory. 


	START_NOT_STICKY
	If the system kills the service after onStartCommand() returns, do not recreate the service unless there are pending intents to deliver. This is the safest option to avoid running your service when not necessary and when your application can simply restart any unfinished jobs.

	START_STICKY
	If the system kills the service after onStartCommand() returns, recreate the service and call onStartCommand(), but do not redeliver the last intent. Instead, the system calls onStartCommand() with a null intent unless there are pending intents to start the service. In that case, those intents are delivered. This is suitable for media players (or similar services) that are not executing commands but are running indefinitely and waiting for a job.

	START_REDELIVER_INTENT
	If the system kills the service after onStartCommand() returns, recreate the service and call onStartCommand() with the last intent that was delivered to the service. Any pending intents are delivered in turn. This is suitable for services that are actively performing a job that should be immediately resumed, such as downloading a file.


Broadcast receivers: Là component cho phép broadcast 1 event đi toàn bộ hệ thống hay trong ứng dụng đang chạy.

Content providers: là component cung cấp 1 interface để quản lý dữ liệu của ứng dụng, thông qua content provider chúng ta có thể chỉnh sửa, truy xuất dữ liệu của ứng dụng khác nếu dc cho phép


Intent filters: cho phép 1 activity được khởi động dựa trên các implicit request.

--------------------------

Activity lifecycle

onCreate()

onStart(): the activity becomes visible to the user. This callback contains what amounts to the activity’s final preparations for coming to the foreground and becoming interactive.

onResume(): invokes this callback just before the activity starts interacting with the user

onPause(): technically means your activity is still partially visible. You should not use onPause() to save application or user data, make network calls, or execute database transactions.

onStop(): the activity is no longer visible to the user

onRestart(): invokes this callback when an activity in the Stopped state is about to restart

onDestroy(): invokes this callback before an activity is destroyed.


Task is a stack of activities


Lauch Mode:
Single Top: thay vì start cùng 1 activity nhiều lần (Vd: trong search page and search again multiple) nhiều instance của activity sẽ dc khởi tạo, sử dụng single top để loại bỏ trường hợp này. Khi sử dụng single top, onNewIntent() sẽ dc gọi. Hãy lấy data bundle trong đây.

Lưu ý single top chỉ ko khởi tạo instance khi activity on top of task
A-B-C-D, D on top và start activity D

Trường hợp start activity B thì new instance vẫn dc khởi tạo


Single task: Khởi tạo activity và add to top, tuy nhiên nếu có tồn tại 1 instance của activity trong 1 task khác thì app sẽ tự navigate qua đó

Single instance: giống như single task nhưng khi start activity xong, system sẽ không launch bất cứ activity nào vào task đó nữa, những activity nào dc start từ activity single instance đều năng trong 1 task khác.


Fragment: Như là 1 module trong activity, 1 activity có thể add nhiều fragment, remove hay replace fragment. Fragment cũng có life cycle riêng của nó	


Loader support load data từ content provider hay 1 nguồn lưu trữ dữ liệu khác 
the problems you might encounter without loaders:

If you fetch the data directly in the activity or fragment, your users will suffer from lack of responsiveness due to performing potentially slow queries from the UI thread.
If you fetch the data from another thread, perhaps with AsyncTask, then you're responsible for managing both the thread and the UI thread through various activity or fragment lifecycle events, such as onDestroy() and configurations changes.


Content provider: là component hỗ trợ quản lý truy xuất dữ liệu của 1 ứng dụng. Thông thường có 2 tình huống nên sử dụng content provider
	1. Ứng dụng muốn truy xuất dữ liệu từ 1 ứng dụng khác có public dữ liệu thông qua content provider
	2. Ứng dụng muốn chia sẻ dữ liệu với những ứng dụng khác.


Content URi: là uri được sử dụng để định danh content provider. khi một ứng dụng call 1 uri đúng với format content uri mà ứng dụng đã đăng kí thì sẽ tiến hành match uri và thực hiện các tác vụ tương ứng

Sử dụng content provider của ứng dụng khác 

					Define Permission in provider app's AndroidManifest.xml

					<permission
					    android:name="com.myapp.PERMISSION"/>
					Define Provider in provider app's AndroidManifest.xml

					<provider
					        android:name=".MyProvider"
					        android:authorities="com.myapp.MyProvider.AUTHORITY"
					        android:enabled="true"
					        android:exported="true"
					        android:multiprocess="true"
					        android:readPermission="com.myapp.PERMISSION" />
					Client's AndroidManifest.xml should have uses-permission tag

					<uses-permission android:name="com.myapp.PERMISSION"/>
					Then client can access the provider

					Cursor cursor = getContentResolver().query(
					Uri.parse("content://com.myapp.MyProvider.AUTHORITY/xxx" ),null, null, null, null);


Broadcast receiver: là 1 android component hỗ trợ nhận đăng kí event system hay custom. Được notify bởi android system

Life cycle của receiver: Khi mà hàm onReceive kết thúc thì sẽ allow gc recycle receiver

					Trước api level 11, ko thể perform asynchronous operation trong hàm onReceive(), vì khi hàm onReceive() finish, gc sẽ có thể recycle receiver => nên sử dụng service 

					Kể từ API 11 có thể sử dụng hàm goAsync() => returns PendingResult. => sẽ cân nhắc là receiver này vẫn còn alive sẽ allow recycle khi PendingResult.finish() được gọi.

					Ví dụ sử dụng goAsync();
					final PendingResult result = goAsync();
					Thread thread = new Thread() {
					   public void run() {
					      int i;
					      // Do processing
					      result.setResultCode(i);
					      result.finish();
					   }
					};
					thread.start();


intent vs pending intent

			Intent là một message được truyền đi giữa các component với nhau (Activities, Services, and BroadcastReceivers). Hay giữa các ứng dụng với nhau.

			Pending intent thường được sử dụng để thực thi một action trong tương lai. là 1 token cho phép pass một intent đến 1 ứng dụng khác với permission của ứng dụng hiện tại

			Một trong những lý do sử dụng pending intent là do intent phải dc create và chạy với 1 cái context valid, nhưng cũng có 1 số trường hợp ở thời điểm trong tương lai context valid ko có sẵn (hay outside application context, ví dụ trong trường hợp notification, lúc ứng dụng ko dc mở). Thì pending sẽ cho phép thực hiện điều đó ở thời điểm hiện tại khi context valid đang có sẵn

Strong, weak references:

			Java has by default 4 types of references: strong, soft, weak and phantom.

			Strong reference: strong references are the ordinary references in Java. Anytime we create a new object, a strong reference is by default created. For example, when we do:

			MyObject object = new MyObject();


			WeakReference: a weak reference is a reference not strong enough to keep the object in memory.

			public class MainActivity extends Activity {
			    @Override
			    protected void onCreate(Bundle savedInstanceState) {
			        super.onCreate(savedInstanceState);
			        new MyAsyncTask(this).execute();
			    }
			    private static class MyAsyncTask extends AsyncTask {
			        private WeakReference<MainActivity> mainActivity;    
			        
			        public MyAsyncTask(MainActivity mainActivity) {   
			            this.mainActivity = new WeakReference<>(mainActivity);            
			        }
			        @Override
			        protected Object doInBackground(Object[] params) {
			            return doSomeStuff();
			        }
			        private Object doSomeStuff() {
			            //do something to get result
			            return new Object();
			        }
			        @Override
			        protected void onPostExecute(Object object) {
			            super.onPostExecute(object);
			            if (mainActivity.get() != null){
			                //adapt contents
			            }
			        }
			    }
			}

			Lưu ý phải sử dụng static inner class để revert memory leak

			Non-static inner classes holds a reference to the containing class. When you declare AsyncTask as an inner class, it might live longer than the containing Activity class. This is because of the implicit reference to the containing class. This will prevent the activity from being garbage collected, hence the memory leak.


Deep link trong ứng dụng android:

Khai báo nhận deep link schema trong file manifest

<data android:scheme="http"
              android:host="www.example.com"
              android:pathPrefix="/gizmos" />

Ứng dụng bên ngoài gọi đúng đường link đã đăng kí nhận
