import okhttp3.*;
import okhttp3.logging.HttpLoggingInterceptor;
import java.io.IOException;
import java.net.InetSocketAddress;
import java.net.Proxy;
import java.util.concurrent.TimeUnit;

public class OkHttpProxyExample {
    public static void main(String[] args) {
        // API URL
        String url = "https://login.microsoftonline.com/106bdeea-f616-4dfc-bc1d-6cbbf45e2011/oauth2/token";

        // Proxy Configuration
        String proxyHost = "tpc-proxy.bnymellon.net";
        int proxyPort = 8080;
        
        // Use HTTP Proxy (not HTTPS)
        Proxy proxy = new Proxy(Proxy.Type.HTTP, new InetSocketAddress(proxyHost, proxyPort));

        // Enable detailed logging (for debugging)
        HttpLoggingInterceptor logging = new HttpLoggingInterceptor();
        logging.setLevel(HttpLoggingInterceptor.Level.BODY);

        // OkHttpClient with Proxy and Logging
        OkHttpClient client = new OkHttpClient.Builder()
                .proxy(proxy)
                .proxyAuthenticator((route, response) -> {
                    // Some proxies require an empty "Proxy-Authorization" header
                    return response.request().newBuilder()
                            .header("Proxy-Authorization", "")
                            .build();
                })
                .addInterceptor(logging)
                .connectTimeout(30, TimeUnit.SECONDS)
                .readTimeout(30, TimeUnit.SECONDS)
                .build();

        // Request Body (application/x-www-form-urlencoded)
        String requestBodyString = "client_id=b930264d-c5a2-469d-ab9a-c1e868dc1f04" +
                                   "&client_secret=g4m8Q-AyOGYvd1i5WbG7g5X9NFi4KBIf.6vGbajn" +
                                   "&grant_type=client_credentials" +
                                   "&resource=https://bny-d36-uat.crm.dynamics.com/" +
                                   "&tenantId=d1783452-59dc-4cf5-81f0-3d4e31b151cb";

        RequestBody requestBody = RequestBody.create(
                MediaType.parse("application/x-www-form-urlencoded"), 
                requestBodyString
        );

        // Create HTTP Request
        Request request = new Request.Builder()
                .url(url)
                .post(requestBody)
                .header("Content-Type", "application/x-www-form-urlencoded")
                .header("Accept", "application/json")
                .build();

        // Execute Request and Print Response
        try (Response response = client.newCall(request).execute()) {
            if (!response.isSuccessful()) {
                throw new IOException("Unexpected code " + response);
            }
            System.out.println("Response: " + response.body().string());
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
