#<표 디자인 하기>#

- - -

###+Inflate 방식을 이용하기###

####1. adapter 만들기####

**1) [adapter레이아웃 파일명].xml**
 
   - `extends BaseAdapter` 를 클래스 명 뒤에 추가해 주어야한다.

   - 소스를 들어가기 전에 ArrayList의 정의에 쓰일 클래스가 하나 필요하다.

   - 이부분은 [adpater].java 가 아닌 `[activity].java`에 추가해준다.

   - ArrayList에 저장해야 할 값들의 변수를 미리 선언해준다.

   - 처음엔 에러라고 뜨지만 import를 해당 activity에 있는 아래의 클래스를 해준다.

    		class use_info{
				public String div;
				public String time;
				public String pay;
		
				public use_info(String div,String time,String pay){
					this.div=div;
					this.time=time;
					this.pay=pay;
				}
			}
     
     
**2) [adapter파일명].java 소스 전체**

			public class recent_use_adapter extends BaseAdapter {

				public ArrayList<use_info> use_list;
				LayoutInflater mLayoutInflater;
				public recent_use_adapter(Context context) {
					super();
					use_list = new ArrayList<use_info>();
					mLayoutInflater = LayoutInflater.from(context);
				}	
				@Override
				public int getCount() {
					return use_list.size();
				}

				@Override
				public Object getItem(int position) {
					return use_list.get(position);
				}

				@Override
				public long getItemId(int position) {
					return position;
				}

				@Override
				public View getView(int position, View convertView, ViewGroup parent) {
					ViewHolder viewHolder;
                    // 캐시된 뷰가 없을 경우 새로 생성하고 뷰홀더를 생성한다
					if(convertView == null) {
						convertView = mLayoutInflater.inflate(R.layout.recent_use_list, parent, false);
						viewHolder = new ViewHolder();
						viewHolder.on_off_div = (TextView)convertView.findViewById(R.id.div_list);
						viewHolder.time = (TextView)convertView.findViewById(R.id.time_list);
						viewHolder.pay = (TextView)convertView.findViewById(R.id.pay_list);
			convertView.setTag(viewHolder);
					} else { // 캐시된 뷰가 있을 경우 저장된 뷰홀더를 사용한다
						viewHolder = (ViewHolder)convertView.getTag();
					}
		
					use_info use_list_info = use_list.get(position);
					viewHolder.on_off_div.setText(use_list_info.div);
					viewHolder.time.setText(use_list_info.time);
					viewHolder.pay.setText(use_list_info.pay);

					return convertView;
				}
	
				static class ViewHolder {
					TextView on_off_div;
					TextView time;
					TextView pay;
				}

			}
            
            
**3) java 소스 설명**

- [activit.java]에 선언해주었던 class를 이용해서 ArrayList 생성

- LayoutInflater를 이용해서 adapter를 불러들인 activity에 삽입

- 다른 부분보다 getView 부분이 중요함.

- ViewHolder 

	- 뷰들을 홀더에 꼽아놓듯이 보관하는 객체. 
	- 각각의 Row를 그려낼때 그 안의 위젯들의 속성을 변경하기 위해서는 findViewById를 호출해야하는데 이것이 비용이 크기때문에 사용.
	- 전체가 public으로 구현된 간단 클래스가 있으면 됨.
		
- inflate할 때는 adapter를 위해 만들어 놓은 xml을 불러온다.

- 처음에 선언은 한번만 하고 setText로 ArrayList의 위치를 하나씩 옮겨가며 값만 바꾸어 주게 됨.

- - -

####2. activity 파일 만들기####

**1) [activity레이아웃 파일명].xml**
    
   - 간단하게 ListView만 추가해주면 된다.


**2)  [activity파일명].java 소스 일부**
 
 - oncreate 부분이 중요

 		 	@Override
			protected void onCreate(Bundle savedInstanceState) {
				super.onCreate(savedInstanceState);
				setContentView(R.layout.recent_use);
				recent_use_adapter UseAdapter = new recent_use_adapter(this);
				use_info temp_use = null;
				ListView recent_listview = (ListView)findViewById(R.id.recent_use);
				recent_listview.setAdapter(UseAdapter);	
		
				StrictMode.setThreadPolicy(new StrictMode.ThreadPolicy.Builder()
				// 스레드 가능
				.detectDiskReads().detectDiskWrites().detectNetwork()
				.penaltyLog().build());

				HttpPostData(); // 웹서버 연결 메소드
		
				for (int i=0; i< result_array.size(); i++) {
					StringTokenizer tokened = new StringTokenizer(result_array.get(i), ",");
					int numberOfToken = tokened.countTokens();
			
					for (int j = 0; j < numberOfToken; j++) {
						recentInfo[i][j] = tokened.nextToken(); // log 테이블에서 지정년도의 월수에  따른 금액 합
				
					}
				}
				for (int i = 0; i < result_array.size(); i++) {
					String temp_div=recentInfo[i][0];
					if(temp_div.equalsIgnoreCase("0")){
						temp_use = new use_info("하차",recentInfo[i][1],recentInfo[i][2]);
					}else if(temp_div.equalsIgnoreCase("1")){
						temp_use = new use_info("승차",recentInfo[i][1],recentInfo[i][2]);
					}else if(temp_div.equalsIgnoreCase("2")){
						temp_use = new use_info("환승",recentInfo[i][1],recentInfo[i][2]);
					}
					UseAdapter.use_list.add(temp_use);
				}
			}
            
            
**3) [activity파일명].java 소스 설명**
 
 - Adapter를 선언. 
 `recent_use_adapter UseAdapter = new recent_use_adapter(this);`
 
 - activity.xml에 존재하는 ListView 선언. 
 `ListView recent_listview = (ListView)findViewById(R.id.recent_use);`
 
 - 선언한 ListView에 setAdapter로 adapter 연결.
 `recent_listview.setAdapter(UseAdapter);`
 
 - 값을 보관하는 객체 생성.
 `temp_use = new use_info("하차",recentInfo[i][1],recentInfo[i][2]);`
 
 - adapter 내부에 선언해주었던 ArrayList에 삽입할 객체들 add.
 `UseAdapter.use_list.add(temp_use);`
 
