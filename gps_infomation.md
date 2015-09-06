#GPS 정보 가져오기#

- 소스 url : [GpsInfo.java](http://https://github.com/sevboo/final/blob/master/src/com/example/project_idfind/GpsInfo.java)

- 네트워크를 이용한 위치정보를 가져오는 방법이 있을거라고 생각하고 찾았지만,
	네트워크를 이용하는 방법도 gps를 켜야만 가능한 것이었음.
    
- 그래서 gps 정보만 이용하는 것이 아니라 gps와 네트워크를 함께 사용하여 좀 더 정확도를 높이는 방법을 사용함.

		locationManager = (LocationManager) mContext
                    .getSystemService(LOCATION_SERVICE);
  
            // GPS 정보 가져오기 
            isGPSEnabled = locationManager
                    .isProviderEnabled(LocationManager.GPS_PROVIDER);
  
            // 현재 네트워크 상태 값 알아오기 
            isNetworkEnabled = locationManager
                    .isProviderEnabled(LocationManager.NETWORK_PROVIDER);
  
            if (!isGPSEnabled && !isNetworkEnabled) {
                // GPS 와 네트워크사용이 가능하지 않을때 소스 구현
            } else {
                this.isGetLocation = true;
                // 네트워크 정보로 부터 위치값 가져오기 
                if (isNetworkEnabled) {
                    locationManager.requestLocationUpdates(
                            LocationManager.NETWORK_PROVIDER,
                            MIN_TIME_BW_UPDATES,
                            MIN_DISTANCE_CHANGE_FOR_UPDATES, this);
                    
                    if (locationManager != null) {
                        location = locationManager
                                .getLastKnownLocation(LocationManager.NETWORK_PROVIDER);
                        if (location != null) {
                            // 위도 경도 저장 
                            lat = location.getLatitude();
                            lon = location.getLongitude();
                        }
                    }
                }
                 
                if (isGPSEnabled) {
                    if (location == null) {
                        locationManager.requestLocationUpdates(
                                LocationManager.GPS_PROVIDER,
                                MIN_TIME_BW_UPDATES,
                                MIN_DISTANCE_CHANGE_FOR_UPDATES, this);
                        if (locationManager != null) {
                            location = locationManager
                                    .getLastKnownLocation(LocationManager.GPS_PROVIDER);
                            if (location != null) {
                                lat = location.getLatitude();
                                lon = location.getLongitude();
                            }
                        }
                    }
                }
            }
            
- gps가 설정되어있지 않다면 알림창이 뜨게되는데 그부분에서 cancle을 눌러도 어플이 계속 될수있는 문제점을 발견.

- `android.os.Process.killProcess(android.os.Process.myPid() ); ` 을 추가하여 어플이 꺼지도록 설정함.

- gps 정보를 처음에 menu화면에 진입하면 위치정보를 저장하도록 되어있었는데,
	정확도면에서 Beacon의 감지가 되는 시점에서의 위치정보를 저장하는 것이 맞다고 판단함.
    
   
- 그래서 gps 값을 받아오는 위치를 backgroundservice.java 부분으로 이동함.

		//선언부
        GpsInfo gps;
        String lati;
        String longi;
        
        //oncreate()부분
        gps=new GpsInfo(BackgroundRangingService.this);
        
        //값 불러오는 부분
        lati = String.valueOf(gps.getLatitude()); //위도
	    longi = String.valueOf(gps.getLongitude()); //경도
