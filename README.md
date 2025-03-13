 public static void main(String[] args) throws IOException {
        OkHttpClient client = new OkHttpClient();

        // Replace with your token URL
        String tokenUrl = "https://your-auth-server.com/oauth/token";

        // Define request body
        RequestBody body = new FormBody.Builder()
                .add("grant_type", "client_credentials")
                .add("client_id", "YOUR_CLIENT_ID")
                .add("client_secret", "YOUR_CLIENT_SECRET")
                .build();

        // Build request
        Request request = new Request.Builder()
                .url(tokenUrl)
                .post(body)
                .addHeader("Content-Type", "application/x-www-form-urlencoded")
                .addHeader("Accept", "application/json")
                .build();

        // Execute request
        Response response = client.newCall(request).execute();
        String responseBody = response.body().string();

        // Print response
        System.out.println("Response: " + response.code());
        System.out.println("Body: " + responseBody);
    }
