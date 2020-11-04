### 20201104 sheetjs를 사용한 표-엑셀 다운(ajax사용).md

다운받을 데이터를 json 형식으로 받아서 아래 내용으로 돌려막기 한다.(자바스크립트 기반이기 때문에 큰 데이터는 힘듦.)

아래 내용으로 구현한 이유는 ajax 사용 - 바로 다운로드 폴더로 저장이 안되고 경로를 지정해줘야 하기 때문에 sheetjs라는 javascript 라이브러리를 사용해서 구현했다.

```javascript
$('#downloadList').click(function(){
    ajax.post('/download/')
        .done(function(r) {
        if(r['RESULT'] === 'SUCCESS' && r['MSG'].length > 0) {
            exportExcel(r.map, r.list);
        } else {
            alert(_MSG['메세지_저장실패']);
        }
    });
})

function s2ab(s) { 
    var buf = new ArrayBuffer(s.length); //convert s to arrayBuffer
    var view = new Uint8Array(buf);  //create uint8array as viewer
    for (var i=0; i<s.length; i++) view[i] = s.charCodeAt(i) & 0xFF; //convert to octet
    return buf;    
}
function exportExcel(data, list){ 
    // step 1. workbook 생성
    var wb = XLSX.utils.book_new();
    // step 2. 시트 만들기 
    var newWorksheet = excelHandler.getWorksheet(data.list);

    // step 3. workbook에 새로만든 워크시트에 이름을 주고 붙인다.  
    XLSX.utils.book_append_sheet(wb, newWorksheet, data.sheetNames);

    // step 4. 엑셀 파일 만들기 
    var wbout = XLSX.write(wb, {bookType:'xlsx',  type: 'binary'});

    // step 5. 엑셀 파일 내보내기 
    saveAs(new Blob([s2ab(wbout)],{type:"application/octet-stream"}), data.excelFileName);
}
var excelHandler = {
        getWorksheet : function(excelData){
            return XLSX.utils.json_to_sheet(excelData);
        }
}
```
