 public static void main(String[] args) throws IOException {
        String url = "https://login.microsoftonline.com/106bdeea-f616-4dfc-bc1d-6cbbf45e2011/oauth2/token";
        String proxyHost = "tpc-proxy.bnymellon.net";
        int proxyPort = 8080;
        
        String clientId = "b930264d-c5a2-469d-ab9a-c1e868dc1f04";
        String clientSecret = "g4m8Q-AyOGYvd1i5WbG7g5X9NFi4KBIf.6vGbajn";
        String grantType = "client_credentials";
        String resource = "https://bny-d36-uat.crm.dynamics.com/";
        String tenantId = "d1783452-59dc-4cf5-81f0-3d4e31b151cb";
        
        Proxy proxy = new Proxy(Type.HTTP, new InetSocketAddress(proxyHost, proxyPort));
        
        OkHttpClient client = new OkHttpClient.Builder()
                .proxy(proxy)
                .connectTimeout(30, TimeUnit.SECONDS)
                .readTimeout(30, TimeUnit.SECONDS)
                .build();
        
        String requestBodyString = "client_id=" + clientId +
                                   "&client_secret=" + clientSecret +
                                   "&grant_type=" + grantType +
                                   "&resource=" + resource +
                                   "&tenantId=" + tenantId;

        RequestBody requestBody = RequestBody.create(MediaType.parse("application/x-www-form-urlencoded"), requestBodyString.getBytes(StandardCharsets.UTF_8));
        
        Request request = new Request.Builder()
                .url(url)
                .post(requestBody)
                .header("Content-Type", "application/x-www-form-urlencoded")
                .header("Accept", "application/json")
                .build();
        
        try (Response response = client.newCall(request).execute()) {
            if (!response.isSuccessful()) throw new IOException("Unexpected code " + response);
            System.out.println("Response: " + response.body().string());
        }
    }
