#### geoserver 범례(getLegendGraphic) 바꿔서 가져와야할 때.md

------

GIS 서비스를 구현하다보면 범례를 가져와야할 때가 있는데 스타일을 바꾸는 등의 작업으로 계속 범례 이미지를 변경해서 가져와야할 때가 있다. 

그럴 때 geoserver에서 제공해주는 getLegendGraphic 서비스 URL로 요청을 해서 이미지를 얻어오면 되는데, 계속해서 요청할 때 같은 URL로 취급해 변경이 안되는 경우가 있다. 이 때 TimeStamp 를 파라미터로 붙여주면 새로운 URL로 인식해서 변경된 URL로 가져올 수 있다. 

`'/geoserver주소/wms?REQUEST=GetLegendGraphic&VERSION=1.0.0&FORMAT=image/png&WIDTH=20&HEIGHT=20&LAYER==레이어명&STYLE=레이어스타일명&n=' + new Date().getTime();`
