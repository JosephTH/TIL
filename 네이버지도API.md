## 네이버 지도 API 사용법

1. 네이버 지도 API 가입(<https://developers.naver.com/docs/map/overview/>)
2. api 추가

```javascript
<script type="text/javascript" src="https://openapi.map.naver.com/openapi/v3/maps.js?clientId=LwR3ey2mwVqqzjczO_Og&callback=initMap"></script>
```

1. 맵 생성

```html
<!--html-->
<div id="map" style="width:100%;height:600px;">
</div>
```

```javascript
//javascript
var map = new naver.maps.Map(document.getElementById('map'));
```

or

```javascript
//javascript
var map = new naver.maps.Map('map');
```

3. 중심 이동, 줌변경

```javascript
var jeju = new naver.maps.LatLng(33.3590628, 126.534361); //위도, 경도로 좌표 설정

map.setCenter(jeju); // 중심 좌표 이동
map.setZoom(13);     // 줌 레벨 변경
```

4. 지도 컨트롤 추가

```javascript
var mapOptions = {
	center: jeju,
	zoom: 10,
    scaleControl: false, //스케일 보여주기
//    scaleControlOptions: {
//        position: naver.maps.Position.BOTTOM_RIGHT
//    },
    logoControl: false,  //네이버 로고 보여주기
    mapTypeControl: false, //위성,일반 구분 기능
    zoomControl: true,    //줌 컨트롤
//    zoomControlOptions: {
//        position: naver.maps.Position.RIGHT_BOTTOM
//     }
    minZoom: 1,
    maxZoom: 10
};

var map = new naver.maps.Map(document.getElementById('map'), mapOptions);
```

5. 네이버로 이동 & 네이버 주소에도 마커 추가, 축약어 N 가능

```javascript
var naver = new N.LatLng(37.3595704, 127.105399);  //1. 좌표
map.setCenter(naver);
var marker = new N.Marker({  //2. 좌표에 마커 찍기
    position: naver,
    map:map
  });
```

8. 마커에 인포 추가&하나 더 추가

```javascript
var contentString = [
    '<div class="iw_inner">',
    '   <h3>서울특별시청</h3>',
    '   <p>서울특별시 중구 태평로1가 31 | 서울특별시 중구 세종대로 110 서울특별시청<br>',
    '       <img src="./img/hi-seoul.jpg" width="55" height="55" alt="서울시청" class="thumb" /><br>',
    '       02-120 | 공공,사회기관 > 특별,광역시청<br>',
    '       <a href="http://www.seoul.go.kr" target="_blank">www.seoul.go.kr/</a>',
    '   </p>',
    '</div>'
].join('');  //3. 정보에 들어갈 내용

var infowindow = new N.InfoWindow({
    content: contentString
}); //4. 정보창에 내용 넣기

N.Event.addListener(marker, "click", function(e) {
    if (infowindow.getMap()) {
        infowindow.close();
    } else {
        infowindow.open(map, marker);
    }
}); //5. 마커와 정보창 연결하기





//하나 더 해보기
var cityhall = new naver.maps.LatLng(37.5666805, 126.9784147); //1. 좌표
var marker2 = new naver.maps.Marker({ //2. 좌표에 마커 찍기
    map: map,
    position: cityhall
});

var contentString2 = [
    '<div class="iw_inner">',
    '   <h3>서울특별시청</h3>',
    '   <p>서울특별시 중구 태평로1가 31 | 서울특별시 중구 세종대로 110 서울특별시청<br>',
    '       <img src="./img/hi-seoul.jpg" width="55" height="55" alt="서울시청" class="thumb" /><br>',
    '       02-120 | 공공,사회기관 > 특별,광역시청<br>',
    '       <a href="http://www.seoul.go.kr" target="_blank">www.seoul.go.kr/</a>',
    '   </p>',
    '</div>'
].join(''); //3. 정보에 들어갈 내용

var infowindow2 = new N.InfoWindow({  //4. 정보창에 내용 채우기
    content: contentString2
});

N.Event.addListener(marker2, "click", function(e) {
    if (infowindow.getMap()) {
        infowindow.close();
    } else {
        infowindow.open(map, marker);
    } 
}); //5. 마커와 정보창 연결

```

9. 멀티플 마커&인포 추가

```javascript
var contentString = '<a href="http://www.naver.com">제주도!</a>'; 
var contentString2 = '<a href="http://www.naver.com">네이버</a>'; 
var contentString3 = '<a href="http://www.daum.net.com">서울시청</a>';

var infowindow = new N.InfoWindow({
    content: contentString
});

var infowindow2 = new N.InfoWindow({
    content: contentString2
});

var infowindow3 = new N.InfoWindow({
    content: contentString3
});


var infoWindows = [infowindow, infowindow2, infowindow3];


var marker = new N.Marker({
    position: jeju,
    map: map
});

var marker2 = new N.Marker({
    position: naver,
    map: map
});

var marker3 = new N.Marker({
    position: cityhall,
    map: map
})

var markers = [marker, marker2, marker3];

function getClickHandler(seq) {
    return function(e) {
        var marker = markers[seq],
            infoWindow = infoWindows[seq];

        if (infoWindow.getMap()) { //정보가 맵에 표시되어 있는지
            infoWindow.close();
        } else {
            infoWindow.open(map, marker);
        }
    }
}

for (var i=0, ii=markers.length; i<ii; i++) {
    N.Event.addListener(markers[i], 'click', getClickHandler(i));
}

```

10. 실습

```javascript
  var data = [
    {name: "아리차이", address: "서울특별시 관악구 신림동길 4", link: "https://store.naver.com/restaurants/detail?id=11704746", x: 126.9282765, y: 37.4876462},
    {name: "한우등촌골", address: "서울특별시 강서구 등촌로 201", link: "https://store.naver.com/restaurants/detail?id=35966485", x:126.86273, y: 37.5476847},
    {name: "수유부추곱창", address: "서울특별시 강북구 도봉로87길 26-21", link: "https://store.naver.com/restaurants/detail?id=37111836", x: 127.0247721, y: 37.6393168},
    {name: "강천민물장어", address:"서울특별시 강동구 올림픽로 834 한강시티라이프", link: "https://store.naver.com/restaurants/detail?id=18905396", x: 127.1297937, y: 37.5553916 }
  ];

//기존 코드와 네이버 API보고 이부분 채우기. contentString format은 제공

/*
  var markers = [];
  var infoWindows = [];

  //marker&infowindow 생성
  for (var i=0; i<data.length; i++) {
    restaurant = data[i]

    var marker = new naver.maps.Marker({
            map: map,
            position: new naver.maps.LatLng(restaurant.y, restaurant.x)
        });

    markers.push(marker);

    var contentString = [
      '<div class="iw_inner">',
      '<h3>'+restaurant.name+'</h3>',
      '<p>'+restaurant.address+'</p>',
      '<a href='+restaurant.link+' target="_blank">알아보기</a>'
    ].join('')

    var infowindow = new naver.maps.InfoWindow({
        content: contentString
    });

    infoWindows.push(infowindow);
   };
*/

function getClickHandler(seq) {
    return function(e) {
        var marker = markers[seq],
            infoWindow = infoWindows[seq];

        if (infoWindow.getMap()) { //정보가 맵에 표시되어 있는지
            infoWindow.close();
        } else {
            infoWindow.open(map, marker);
        }
    }
}

for (var i=0, ii=markers.length; i<ii; i++) {
    N.Event.addListener(markers[i], 'click', getClickHandler(i));
}
```

11. 마커 이미지 교환

```javascript
    var marker = new N.Marker({
            map: map,
            position: new N.LatLng(restaurant.y, restaurant.x),
            icon: {
              url: 'https://navermaps.github.io/maps.js/docs/img/example/sally.png',
              size: new N.Size(50, 52),  //이미지 크기는 변함x, 보여지는 div크기만 변함
              origin: new N.Point(0, 0), //이미지를 이동, 보여지는 div는 고정
              anchor: new N.Point(25, 26) //div자체를 이동
            }
        	//animation: 1 or 2 //bounce or drop
        });
```

12. 마커 스코프 로딩

```javascript
N.Event.addListener(map, 'idle', function() { //지도의 움직임이 종료되었을 때(유휴 상태) 이벤트가 발생합니다. Class: naver.maps.Map > Events 참조
    updateMarkers(map, markers);
});

function updateMarkers(map, markers) {

    var mapBounds = map.getBounds(); //맵의 둘레 좌표 반환. 정확힌 남서쪽과 북동쪽 좌표.
    var marker, position;

    for (var i = 0; i < markers.length; i++) {

        marker = markers[i]
        position = marker.getPosition();

        if (mapBounds.hasLatLng(position)) { //position이 경계 안에 있는지 check
            showMarker(map, marker);
        } else {
            hideMarker(map, marker);
        }
    }
}

function showMarker(map, marker) {

    if (marker.getMap()) return; //마커가 화면 위에 있으면 아무것도 하지 않고
    marker.setMap(map);			 //마커가 화면 위에 없으면 화면 위에 띄워줌
}

function hideMarker(map, marker) { 

    if (!marker.getMap()) return;//마커가 화면 위에 없으면 넘어가고
    marker.setMap(null);         //마커가 화면 위에 있으면 없애줌
}

```

14. 한국 학교 실습! https://docs.google.com/spreadsheets/d/1UPUU_-KYpQ1FlhYRlISIH7YAcuNCAMVl90XGxCc8p5w/edit?usp=drive_web

```ruby
#rails g model School name:string address:string lng:float lat:float
#seeds.rb
CSV.foreach(Rails.root.join("school.csv"), headers: true) do |row|
  School.create! row.to_hash
end

#rake db:migrate
#rake db:seed


#controller
def map_data
    @school = School.all.limit(10); #limit 늘려가면서 실행
    respond_to do |format|
        format.json {render json: @school} #rails c => School.first.to_json
end
    
#routes.rb
get 'map_data' => 'posts#map_data'
```

```javascript
var data;
var markers = [];
var infoWindows = [];
getData();


function getData() { //1.
    $.ajax({
      url: '/map_data' ,
      dataType: 'json',
      type: "POST",
      data: {authenticity_token: $("[name='csrf-token']").attr('content')},
      success: function(result) {
        data = result;
        alert('success');
        console.log(data);
        createMarkersInfos(); //ajax라서 불러오기 전에는 data가 없음! 불러온 다음에 실행하게 해야함
      }
    });
  }


//원래 있는 애들 중 함수명 붙여준 애들
function createMarkersInfos() { //1. 마커와 인포 생성
  alert('create!');
  for (var i=0; i<data.length; i++) {
    restaurant = data[i]

    var marker = new N.Marker({
            map: map,
            position: new N.LatLng(restaurant.y, restaurant.x),
            icon: {
              url: 'https://navermaps.github.io/maps.js/docs/img/example/sally.png',
              size: new N.Size(50, 52),
              origin: new N.Point(0, 0),
              anchor: new N.Point(0, 0)
            }
        });

    markers.push(marker);

    var contentString = [
      '<div class="iw_inner">',
      '<h3>'+restaurant.name+'</h3>',
      '<p>'+restaurant.address+'</p>',
      '<a href='+restaurant.link+' target="_blank">알아보기</a>'
    ].join('')

    var infowindow = new N.InfoWindow({
        content: contentString
    });

    infoWindows.push(infowindow);
   };
  addEvents(); //마커와 인포가 다 생성되면 이벤트핸들러 붙여주기.
}

function addEvents() { //2. 이벤트 핸들러 붙이기.
  alert('addEvent!');
for (var i=0, ii=markers.length; i<ii; i++) {
    N.Event.addListener(markers[i], 'click', getClickHandler(i));
}

    
    
//원래 있는애들 중 수정 안한 애들
    

function getClickHandler(seq) {
    return function(e) {
        var marker = markers[seq],
            infoWindow = infoWindows[seq];

        if (infoWindow.getMap()) {
            infoWindow.close();
        } else {
            infoWindow.open(map, marker);
        }
    }
}
    
N.Event.addListener(map, 'idle', function() {
    updateMarkers(map, markers);
});
}

function updateMarkers(map, markers) {

    var mapBounds = map.getBounds(); //맵의 둘레 좌표 반환. 정확힌 남서쪽과 북동쪽 좌표.
    var marker, position;

    for (var i = 0; i < markers.length; i++) {

        marker = markers[i]
        position = marker.getPosition();

        if (mapBounds.hasLatLng(position)) { //position이 경계 안에 있는지 check
            showMarker(map, marker);
        } else {
            hideMarker(map, marker);
        }
    }
}

function showMarker(map, marker) {

    if (marker.getMap()) return; //마커가 화면 위에 있으면 아무것도 하지 않고
    marker.setMap(map);			 //마커가 화면 위에 없으면 화면 위에 띄워줌
}

function hideMarker(map, marker) { 

    if (!marker.getMap()) return;//마커가 화면 위에 없으면 넘어가고
    marker.setMap(null);         //마커가 화면 위에 있으면 없애줌
}
```

15. 한국학교 개선 1차(일정 좌표만 불러오기)

```ruby
#controller
def map_data
    max = JSON.parse(params[:max])
    min = JSON.parse(params[:min])
    school = School.where("(x BETWEEN ? and ?) and (y BETWEEN ? and ?)", min["_lng"],max["_lng"],min["_lat"], max["_lat"])
    respond_to do |format|
      format.json {render json: school)}
    end
  end
```

```javascript
//map.html.erb  
function getData() {
    var bounds = map.getBounds();
    var max = bounds._max;
    var min = bounds._min;
    $.ajax({
      url: '/map_data' ,
      data: {max: JSON.stringify(max), min: JSON.stringify(min), authenticity_token: $("[name='csrf-token']").attr('content')},
      dataType: 'json',
      type: "POST",
      success: function(result) {
        data = result[0];
        indice = indice.concat(result[1]);
        //alert('success');
        console.log(data);
        createMarkersInfos();
      }
    });
  }
```

16. 한국학교 개선 2차(기존에 없는 좌표만 불러오기 & 새로 불러온 좌표만 마커 및 인포 생성, 바뀐 부분만 주석처리x)

```javascript
var indice = [];

/*  function getData() {
    var bounds = map.getBounds();
    var max = bounds._max;
    var min = bounds._min;
    $.ajax({
      url: '/map_data' , */
      data: {max: JSON.stringify(max), min: JSON.stringify(min), indice: JSON.stringify(indice), authenticity_token: $("[name='csrf-token']").attr('content')},
/*      dataType: 'json',
      type: "POST",
      success: function(result) { */
        data = result[0];
        indice = indice.concat(result[1]);
        //alert('success');
//        console.log(data);
//        createMarkersInfos();
      }
    });
  }
```

```ruby
  def map_data
 #   max = JSON.parse(params[:max])
 #   min = JSON.parse(params[:min])
    indice = JSON.parse(params[:indice])
 #   school = School.where("(x BETWEEN ? and ?) and (y BETWEEN ? and ?)", min["_lng"],max["_lng"],min["_lat"], max["_lat"])
    school_id = school.map {|x| x.id}
    school_id -= indice
    unless school_id.length == 0
      school = school.select {|x| school_id.include? x.id}
    else
      school = []
    end
#    respond_to do |format|
      format.json {render json: [school, school_id]}
#    end
  end

```



```javascript
function createMarkersInfos() {
  var new_markers = [];
  var new_infoWindows = [];
 /*
  //alert('create!');
  for (var i=0; i<data.length; i++) {
    restaurant = data[i]

    var marker = new N.Marker({
            map: map,
            position: new N.Point(restaurant.x, restaurant.y),
            animation: 2
            /*icon: {
              url: 'https://navermaps.github.io/maps.js/docs/img/example/sally.png',
              size: new N.Size(50, 52),
              origin: new N.Point(0, 0),
              anchor: new N.Point(0, 0)
            }
        });
 */
    new_markers.push(marker);
 /*   markers.push(marker);

    var contentString = [
      '<div class="iw_inner">',
      '<h3>'+restaurant.name+'</h3>',
      '<p>'+restaurant.address+'</p>',
      '<a href='+restaurant.link+' target="_blank">알아보기</a>'
    ].join('')

    var infowindow = new N.InfoWindow({
        content: contentString
    });
*/
    new_infoWindows.push(infowindow);
//   infoWindows.push(infowindow);
//  }; */
  addEvents(new_markers, new_infoWindows);
}


function addEvents(nm, ni) {
  //alert('addEvent!');
for (var i=0, ii=nm.length; i<ii; i++) {
    N.Event.addListener(nm[i], 'click', getClickHandler(nm,ni,i));
//}
//  updateMarkers(map,markers);
//}

function getClickHandler(markers, infoWindows, seq) {
/*    return function(e) {
        var marker = markers[seq],
            infoWindow = infoWindows[seq];

        if (infoWindow.getMap()) { //정보가 맵에 표시되어 있는지
            infoWindow.close();
        } else {
            infoWindow.open(map, marker);
        }
    }
}*/
```

