OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("application/json");
RequestBody body = RequestBody.create(mediaType, "{\r\n    \"securitysystem\": \"System1\",\r\n    \"endpoint\": \"System1\",\r\n    \"username\": \"admin\",\r\n    \"dynamicattributes\": [\r\n        {\r\n            \"accountscolumn\": \"\",\r\n            \"hideoncreate\": \"false\",\r\n            \"attributegroup\": \"\",\r\n            \"orderindex\": \"\",\r\n            \"requesttype\": \"SERVICE ACCOUNT\",\r\n            \"actionstring\": \"\",\r\n            \"editable\": \"true\",\r\n            \"attributename\": \"air5\",\r\n            \"hideonupdate\": \"false\",\r\n            \"actiontoperformwhenparentattributechanges\": \"\",\r\n            \"defaultvalue\": \"\",\r\n            \"attributelable\": \"\",\r\n            \"required\": \"false\",\r\n            \"attributetype\": \"BOOLEAN\",\r\n            \"regex\": \"\",\r\n            \"attributevalue\": \"\",\r\n            \"showonchild\": \"false\",\r\n            \"Parentattribute\": \"\",\r\n            \"descriptionascsv\": \"\"\r\n        }\r\n    ]\r\n}");
Request request = new Request.Builder()
  .url("{{url}}/ECM/{{path}}/createDynamicAttribute")
  .method("POST", body)
  .addHeader("Content-Type", "application/json")
  .addHeader("Authorization", "Bearer {{token}}")
  .build();
Response response = client.newCall(request).execute();
