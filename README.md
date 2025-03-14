import okhttp3.*;
import java.io.IOException;
import java.net.*;

public class OkHttpProxyAuth {
    public static void main(String[] args) throws IOException {
        String url = "https://login.microsoftonline.com/106bdeea-f616-4dfc-bc1d-6cbbf45e2011/oauth2/token";
        String proxyHost = "tpc-proxy.bnymellon.net";
        int proxyPort = 8080;

        // Set up Proxy (SAME as Postman)
        Proxy proxy = new Proxy(Proxy.Type.HTTP, new InetSocketAddress(proxyHost, proxyPort));

        // OkHttp Client with Proxy + Auto Authentication Handler
        OkHttpClient client = new OkHttpClient.Builder()
                .proxy(proxy)
                .proxyAuthenticator((route, response) -> {
                    // IF proxy asks for authentication (407 response), return a new request
                    if (response.code() == 407) {
                        System.out.println("Proxy Authentication Required!");
                        return response.request().newBuilder().build();
                    }
                    return null; // No authentication required
                })
                .build();

        // Postman request body
        String requestBodyString =
                "client_id=b930264d-c5a2-469d-ab9a-c1e868dc1f04" +
                "&client_secret=g4m8Q-AyOGYvd1i5WbG7g5X9NFi4KBIf.6vGbajn" +
                "&grant_type=client_credentials" +
                "&resource=https://bny-d36-uat.crm.dynamics.com/" +
                "&tenantId=d1783452-59dc-4cf5-81f0-3d4e31b151cb";

        RequestBody requestBody = RequestBody.create(
                requestBodyString, MediaType.get("application/x-www-form-urlencoded"));

        // Build Request (EXACTLY LIKE POSTMAN)
        Request request = new Request.Builder()
                .url(url)
                .post(requestBody)
                .addHeader("Content-Type", "application/x-www-form-urlencoded")
                .addHeader("Accept", "application/json")
                .build();

        // Execute request
        try (Response response = client.newCall(request).execute()) {
            System.out.println("Response Code: " + response.code());
            System.out.println("Response Body: " + (response.body() != null ? response.body().string() : "No Response"));
        }
    }
}
