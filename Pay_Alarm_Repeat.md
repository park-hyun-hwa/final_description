#알람 반복하기#

##개념##

- 사용자들에게 결제가 될 것이라고 알림을 보내는 방법을 찾는과정에서 
	푸시 알람을 보내는 방식으로 유명한 `GCM(Google Cloud Messaging)`방식도 있고,
    `AlarmManager` 방식도 있다.
    
- 매번 공지할 내용이 바뀌는 것이 아니기 때문에 매달 반복날짜를 정해놓는 AlarmManager를 사용함.
    
- - -
    
- AlarmManager 란 : 단말기가 SleepMode로 바뀐 상황에서 원하는 시간에 특정작업을 수행하도록 할 수 있는 방법

- AlarmManager의 설정항목
	- 기준이 되는 시간에 따라
		- 단말기가 시작된 이후 : Elapsed Real Time
		- 실제 시간 : RTC(Real-time clock)

	- 단말기의 상태에 따라
    	- 대기상태일 때도 수행할 것인지
    	- 대기상태일 때는 수행하지 않을 것인지


|           속성          |                            설명                            |
|:-----------------------:|:----------------------------------------------------------:|
|     ELAPSED_REALTIME    |               부팅된 이후 경과된 시간을 기준               |
| ELAPSED_REALTIME_WAKEUP |  대기상태일 경우 단말기를 활성상태로 전환한 후 작업을 수행 |
|           RTC           |                      실제 시간을 기준                      |
|        RTC_WAKEUP       | 대기상태일 경우 단말기를 활성 상태로 전환한 후 작업을 수행 |

- 한번 수행 
	 - `public void set (int type, long triggerAtTime, PendingIntent operation)`

- 반복 수행(정확한 시간에)
	 - `public void setRepeating (int type, long triggerAtTime, long interval, PendingIntent operation)`

- 반복 수행(근사한 시간에
	 - `public void setInexactRepeating (int type, long triggerAtTime, long interval, PendingIntent operation)`

* * *

##소스코드##

- 실제 코드는 [여기](https://github.com/sevboo/final/tree/master/src/com/example/project_idfind)에서 `MainActivity.java` 와 `AlarmReceive.java`를 함께 보면 된다.

_ _ _

- MainActivity.java의 일부

	- AlarmService 등록 : `AlarmManager am = (AlarmManager)getSystemService(Context.ALARM_SERVICE);`

	- PendingIntent : Intent 객체를 감싸는 객체로 그것이 포함하고 있는 intent를 외부의 다른 어플리케이션에서 실행할 수 있는 권한을 주는 것.

		- Notification에서 특정 액션을 수행할 때

		- AppWidget에서 특정 액션을 수행할 때

		- 미래의 특정 시점에서 실행될 인텐트를 선언할 때

	- 생성하는 intent에 따라 getActivity(), getService(), getBroadcast() 메소드를 사용한다.
  
		- `PendingIntent sender = PendingIntent.getBroadcast(MainActivity.this, 0, intent, 0);`
  
	- Calendar.getInstance()로 시간을 불러와서 알람시간을 다시 set해준다.
	
	 - `Calendar calendar = Calendar.getInstance();`
	
  - 매 달 같은 날에 알림이 울리게 하기 위해서 간격 속성에 `AlarmManager.INTERVAL_DAY * 30` 로 설정.
  
    - `am.setRepeating(AlarmManager.RTC_WAKEUP, calendar.getTimeInMillis(), AlarmManager.INTERVAL_DAY * 31, sender);`
  
  - 각 달에 맞게 다음달 25일에도 알람이 울릴 수 있도록 am.setRepeating()메소드를 각각 호출한다.
	
	- `setRepeating(int type, long triggerAtTime, long interval, PendingIntent operation)`
    
    

- - -


    
- AlarmReceive.java

	- BroadcastReceiver를 상속받고 onReceive함수를 오버라이드한다.

	- 알람 시간이 되면 onReceive를 호출하게 됨.


- - -


- 추가적으로 이 뿐만 아니라 Manifest.xml에도 추가한다.

	- `<receiver android:name="AlarmReceive" android:process=":remote" />`
