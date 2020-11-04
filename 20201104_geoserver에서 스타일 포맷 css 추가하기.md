### 20201104_geoserver에서 스타일 포맷 css 추가하기.md

```
https://build.geoserver.org/geoserver/
```

여기서 버전에 맞는 버전 클릭해서 Geoserver 설치한 경로에서 webapps/geoserver/WEB-INF/lib에 넣어주면 된다(하위 폴더 생성 ㄴㄴ)

(ex. https://build.geoserver.org/geoserver/master/ext-latest/geoserver-2.19-SNAPSHOT-css-plugin.zip)

내 폴더 경로 ▼

```
C:\Program Files (x86)\GeoServer 2.15.0\webapps\geoserver\WEB-INF\lib
```

~~내가 다운받은 플러그인 버전은 2.16이였다.~~ 

2.16버전 다운받았더니 `Argument "message" should not be null` 이런 에러가 떠서 css로 변경이 안되고 저장도 안되고 유효성검사도 안됨.....그래서 다시 검색해서 2.15.x 버전으로 다운받음

최신버전 사용을 습관화합니다...따흑.....

