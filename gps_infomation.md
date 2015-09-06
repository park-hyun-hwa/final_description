#각종 오류들#

###iconv(): Detected an illegal character in input string###


- DB에 insert하는 과정에서 위와 같은 오류가 발생.

- iconv()에서 인코딩하는 방식이 아래와 같이 되어있었는데,
        
        iconv ( "UTF-8", "EUC-KR", $query2 );
        
- 아래와 같이 바꾸어주니 해결이 됨.
		
        iconv ( "CP949", "UTF-8//TRANSLIT", $query2 );
        
- 왜 그런지는 아직 파악이 안됨.

- 보통 검색결과로는 아래와 같이 하여 무시하는 방법을 쓰라고 하는데 나는 //TRANSLIT를 쓰니 해결이 되었음.
		
        iconv("EUC-KR","UTF-8//IGNORE",$str); 

- 조사결과 ignore 방식과 translit의 차이점은 
  - //IGNORE : Ignore (drop them) characters not recognized
  - //TRANSLIT : Attempt to replace them with approximates
  
_ _ _

###SELECT 결과가 아무것도 없을때는 0으로 간주하게 하는 sql###

- 미결제된 금액을 표시하고자 하려니까 처음에 가입하고 메뉴화면에 입장하였을때는 제대로 값이 찍히지 않는 현상을 발견하였음.

- 그래서 select 결과가 null이 뜨는 것을 0으로 간주하게 하는 sql 문장을 찾음.
	
    	$nonpay_log="select NVL(sum(charge) , 0) as sum from log where custom_id='$mem_id' and pay='0'";

- 위와 같은 식으로 `NVL(찾고자하는 값, null일때 반환하고자하는 값)` 으로 구성해주면 됨.
