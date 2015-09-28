#스와이프 메뉴 만들기#
##다양한 방법##
- 프로젝트를 만들때 Tabbed Activity로 생성하기

	- 좌우 스와이프가 가능한 탭기반의 어플을 만들 수 있다.


- ViewFlipper를 이용하기

	- Tab과 마찬가지로 화면에 표시가능한 여러개의 view가 준비되어 있을 대 각 View와의 전환을 하기 위해서 사용한다.
	- 화면 전체를 원하는 view로 채워 넣을 수 있다.
	- 비순차적으로 건너뛰는 것은 안되고, 순차적으로 이동해야한다.
	- 참고 url : http://tigerwoods.tistory.com/23

- 이외에 Gallery, HorizontalScrollView,ViewSwitcher를 사용하기도 한다.

- 하지만 좀 더 편리하고 기능적인 화면을 구성하기 위해 아래의 방법을 사용한다.

- ViewPager를 이용하기

	- 수평으로 View를 좌/우 스크롤할 때 사용한다.
	- 안드로이드 1.6 버전 이후부터 사용이 가능하다.
	- 구글 마켓이나 구글+도 이 방식을 사용한다.

##ViewPager 사용하기##

- `android-support-v4.jar` 라이브러리를 추가하여 사용해야한다.

####1. Xml에 ViewPager 적용하기####
- view의 이동을 주고 싶은 xml에 아래와 같은 부분을 추가한다.

		<android.support.v4.view.ViewPager
		android:id="@id/viewpager"
		android:layout_width="wrap_content"
		android:layout_height="wrap_content" />
        
####2. Java에서 PageAdapter 생성하기####

- - viewpager를 추가한 java 파일에 아래와 같은 Class를 추가해준다

	- PagerAdapter를 상속받는다.

	- 생성자 이외에 필요한 메소드를 override 한다

		- getCount() : page의 갯수

		- instantiateItem() : getCount를 통해서 얻어온 position 별로 등록할 item을 처리하는 메소드(사용할 view 객체를 생성하는 메소드)

		- isViewFromObject() : instantiateItem메소드에서 생성한 객체를 이용할 것인지 여부를 반환하는 메소드

		- 추가적으로 OnClickListener를 생성하여 각 view 내부의 버튼을 클릭하였을 때 이동하는 화면을 설정한다.

	- 아래는 해당 class의 전체 소스이다.

			private class adapter extends PagerAdapter {
    		private LayoutInflater mInflater;
			public adapter(Context c) {
				super();
				mInflater=LayoutInflater.from(c);
			
			}

			@Override
			public int getCount() {
				return MAX_PAGE;
			}
			@Override
			public Object instantiateItem(View pager,int position){
				View v = null;
				if(position==0){
					v=mInflater.inflate(R.layout.viewpager,null);
					v.findViewById(R.id.user_update_button).setOnClickListener(mButtonClick);
					v.findViewById(R.id.card_info_button).setOnClickListener(mButtonClick);
					v.findViewById(R.id.notice_button).setOnClickListener(mButtonClick);
					v.findViewById(R.id.qna_button).setOnClickListener(mButtonClick);
				}else if(position==1){
					v=mInflater.inflate(R.layout.veiwpager2,null);
					v.findViewById(R.id.pay_button).setOnClickListener(mButtonClick);
					v.findViewById(R.id.pay_list_button).setOnClickListener(mButtonClick);
					v.findViewById(R.id.recent_list_button).setOnClickListener(mButtonClick);
					v.findViewById(R.id.month_list_button).setOnClickListener(mButtonClick);
			}
				((ViewPager)pager).addView(v,0);
				return v;
			}

			@Override
			public boolean isViewFromObject(View v, Object obj) {
				return v == obj;
			}

			}
   	 		private OnClickListener mButtonClick = new OnClickListener(){
    		public void onClick(View v){
    			switch(v.getId()){
    			case R.id.user_update_button:
    				intent = new Intent(MenuActivity.this,InfoModify.class);
    				break;
    			case R.id.card_info_button:
    				intent = new Intent(MenuActivity.this,CardModify.class);
    				break;
    			case R.id.notice_button:
    				intent = new Intent(MenuActivity.this,Notice.class);
    				break;
    			case R.id.qna_button:
    				intent = new Intent(MenuActivity.this,CardModify.class);
    				break;
    			case R.id.pay_button:
    				intent = new Intent(MenuActivity.this,PayActivity.class);
    				break;
    			case R.id.pay_list_button:
    				intent = new Intent(MenuActivity.this,PayUse.class);
    				break;
    			case R.id.recent_list_button:
    				intent = new Intent(MenuActivity.this,RecentUse.class);
    				break;
    			case R.id.month_list_button:
    				intent = new Intent(MenuActivity.this,MonthUse.class);
    				break;
    			}
    			startActivity(intent);
    			}
    		};
            
####3. Main Java 파일에서 ViewPager와 Adapter 연결시키기####

- 추가해준 class를 onCreate에서 ViewPager와 PageAdapter를 setAdapter를 통해서 연결시켜준다.

		ViewPager viewPager=(ViewPager)findViewById(R.id.viewpager); 
        viewPager.setAdapter(new adapter(getApplicationContext()));
        
- 즉, ViewPager가 ListView이고 PagerAdapter가 ListAdapter의 역할과 동일하다.

- 그래서 Adpater를 통해 ViewPager에 표시되는 data를 관리한다.

