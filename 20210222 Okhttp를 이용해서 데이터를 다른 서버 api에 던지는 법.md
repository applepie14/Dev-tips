### Okhttp를 이용해서 데이터를 다른 서버 api에 던지는 법

1. 일반적인 String, Int 등의 데이터 타입을 던질 때

   - Okhttp를 사용해서는 String 타입밖에 못 던진다.

   - 결국 모든 데이터를 String으로 던지고 받는 쪽에서 다시 원하는 데이터 형으로 파싱해주는 수밖에 없다.

   - 아래 방법은 map 형식으로 받아온 정보를 JSON 형식으로 변환해서 보낸다. 이렇게 하면 받는 쪽에서 다시 JSON > MAP으로 변환해야한다.(보통 정보가 많으면 이렇게 보내는게 편하다)

     ```java
     		String paramStr = mapper.writeValueAsString(param); // map으로 받아온 데이터를 json형식의 string으로 변환
     		OkHttpClient client = new OkHttpClient();
     		RequestBody body = new FormBody.Builder().add("param", paramStr).build(); // json형식으로 변환한 데이터들을 body에 실어준다.
     
     		Request request = new Request.Builder().url(url).post(body).build(); // body에 실어준 데이터들을 post형으로 빌드
     
     		Response response = client.newCall(request).execute(); // 저 짝에 데이터 던지기
     		String responseString = response.body().string(); // 결과값을 받는다. 
             int result = Integer.parseInt(responseString); // 결과값은 데이터를 받은 api의 컨트롤러에서 반환해주는 정보를 가지고 온다. 여기서 result는 아마 데이터를 받은 api에서 update처리 또는 insert 처리된 갯수를 반환하기하는 듯
             if(result > 0) {
                 for(int i=0; i< param.size(); i++) {
                     dao.updateTransferWkt(param.get(i));
                 }
             }
     ```

2. 파일을 던질 때

   - 파일은 일반적인 작업처럼 Multipart로 넘기고 받아야한다. 

   - 조금 귀찮음

   - 많이 귀찮음

   - 그래도 해야지 내가 개발잔데....

   - 위의 방식처럼 JSON으로 뭉텅이로 보내는 법은 모르겠어서 하나씩 넘겨줬다. 

     ```java
     OkHttpClient client = new OkHttpClient();
     Builder requestBodyBuilder = new MultipartBody.Builder()
         .setType(MultipartBody.FORM)
         .addFormDataPart("데이터1", param.get("데이터1"))
         .addFormDataPart("originalFl", fileVO.getOriginalFlnm(), RequestBody.create(MultipartBody.FORM, originFile)); //
     
     RequestBody requestBody = requestBodyBuilder.build();
     Request request = new Request.Builder()
                .url(url)
                .post(requestBody)
                .build();
     Response response = client.newCall(request).execute();
     if(response.isSuccessful()) { // isSuccessful() 메소드는 okhttp 자체 기능. 정보를 보내는데 성공했는지 아닌지를 알려주는 듯하다. 
         ResponseBody body = response.body();
         if(body != null) {
             String responseStr = body.string();
             if("true".equals(responseStr)) {
                 dao.updateTranferAt(param);
             }
             result = "success";
         }
     }
     ```

     

   - 파일을 받는 쪽 처리

   - 받는 쪽에 MultipartHttpServletRequest request 이 부분을 적어줘야 파일 확인할 수 있다......몰라서 2일동안 머리 싸맴

     ```java
     @PostMapping(path = "/save", consumes = "multipart/*", produces = "application/json;charset=utf-8")
     public boolean 컨트롤러1(MultipartHttpServletRequest request, @RequestParam Map<String, Object> param) {
         boolean result = 서비스1.save(request, param);
     
         return result;
     }
     ```

     

     
