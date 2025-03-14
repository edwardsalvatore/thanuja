 public static void main(String[] args) throws IOException {
        // API URL from Postman
        String url = "https://login.microsoftonline.com/106bdeea-f616-4dfc-bc1d-6cbbf45e2011/oauth2/token";
        
        // HTTP Proxy from Postman
        Proxy proxy = new Proxy(Proxy.Type.HTTP, new InetSocketAddress("tpc-proxy.bnymellon.net", 8080));

        // OkHttpClient with Proxy (No authentication, matching Postman behavior)
        OkHttpClient client = new OkHttpClient.Builder()
                .proxy(proxy)
                .connectTimeout(30, TimeUnit.SECONDS)
                .readTimeout(30, TimeUnit.SECONDS)
                .build();
        
        // Request Body (Form URL Encoded, matching Postman)
        String requestBodyString = "client_id=b930264d-c5a2-469d-ab9a-c1e868dc1f04" +
                                   "&client_secret=g4m8Q-AyOGYvd1i5WbG7g5X9NFi4KBIf.6vGbajn" +
                                   "&grant_type=client_credentials" +
                                   "&resource=https://bny-d36-uat.crm.dynamics.com/" +
                                   "&tenantId=d1783452-59dc-4cf5-81f0-3d4e31b151cb";
        
        RequestBody requestBody = RequestBody.create(
                MediaType.parse("application/x-www-form-urlencoded"), 
                requestBodyString.getBytes(StandardCharsets.UTF_8)
        );

        // Request with Headers
        Request request = new Request.Builder()
                .url(url)
                .post(requestBody)
                .header("Content-Type", "application/x-www-form-urlencoded")
                .header("Accept", "application/json")
                .build();
        
        // Execute the Request
        try (Response response = client.newCall(request).execute()) {
            if (!response.isSuccessful()) throw new IOException("Unexpected code " + response);
            System.out.println("Response: " + response.body().string());
        }
    }
