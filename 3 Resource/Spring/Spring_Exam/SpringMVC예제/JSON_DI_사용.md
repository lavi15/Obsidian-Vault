
**※ Spring 에서 자동으로 JSON File로 바꿔주기 때문에 사용은 하지 않음**
```java
    public List<UserDTO> getUserList(String pg) {  
        Map<String, String> numMap = new HashMap<>();  
        int endNum = Integer.parseInt(pg)*5;  
        int startNum = endNum-4;  
        numMap.put("startNum", startNum+"");  
        numMap.put("endNum", endNum+"");  
        List<UserDTO> dtoList = userDAO.getUserList(numMap);  
        System.out.println(dtoList);  
  
        JSONObject json = new JSONObject();  
  
        JSONArray array =new JSONArray();  
  
        for(UserDTO dto : dtoList) {  
            JSONObject temp = new JSONObject();  
            temp.put("name", dto.getName());  
            temp.put("id", dto.getId());  
            temp.put("pwd", dto.getPwd());  
            array.put(temp);  
        }        json.put("list", array);  
  
        System.out.println(json);  
  
        return dtoList;  
    }}
```

**결과 값**
```JSON
{"list":[{"name":"코난","id":"perfact","pwd":"1234"},{"name":"홍길동","id":"javamaster","pwd":"12345"},{"name":"라이언","id":"lions","pwd":"1234"},{"name":"hong","id":"1234","pwd":"1234"},{"name":"jang","id":"jang","pwd":"1234"}]}
```
