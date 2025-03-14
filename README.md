String clientId = "your_client_id";
        String clientSecret = "your_client_secret";
        String urlString = "https://your-api-endpoint.com";
        
        Proxy proxy = new Proxy(Proxy.Type.HTTP, new InetSocketAddress("your_proxy_host", your_proxy_port));
        URL url = new URL(urlString);
        HttpURLConnection connection = (HttpURLConnection) url.openConnection(proxy);
        
        String auth = clientId + ":" + clientSecret;
        String encodedAuth = Base64.getEncoder().encodeToString(auth.getBytes());
        connection.setRequestProperty("Authorization", "Basic " + encodedAuth);
        
        connection.setRequestMethod("GET");
        connection.setDoOutput(true);
        
        int responseCode = connection.getResponseCode();
        System.out.println("Response Code: " + responseCode);
