import okhttp3.*;

import java.io.IOException;
import java.util.concurrent.TimeUnit;

public class UseSystemProxy {
    public static void main(String[] args) throws IOException {
        String url = "https://login.microsoftonline.com/106bdeea-f616-4dfc-bc1d-6cbbf45e2011/oauth2/token";

        String requestBodyString =
                "client_id=b930264d-c5a2-469d-ab9a-c1e868dc1f04" +
                "&client_secret=g4m8Q-AyOGYvd1i5WbG7g5X9NFi4KBIf.6vGbajn" +
                "&grant_type=client_credentials" +
                "&resource=https://bny-d36-uat.crm.dynamics.com/" +
                "&tenantId=d1783452-59dc-4cf5-81f0-3d4e31b151cb";

        OkHttpClient client = new OkHttpClient.Builder()
                .connectTimeout(30, TimeUnit.SECONDS)
                .readTimeout(30, TimeUnit.SECONDS)
                .proxy(Proxy.NO_PROXY)  // 🔥 Let Java use system proxy automatically
                .build();

        RequestBody requestBody = RequestBody.create(
                requestBodyString,
                MediaType.parse("application/x-www-form-urlencoded")
        );

        Request request = new Request.Builder()
                .url(url)
                .post(requestBody)
                .header("Content-Type", "application/x-www-form-urlencoded")
                .header("Accept", "application/json")
                .header("User-Agent", "PostmanRuntime/7.32.2")  // 🚀 Use Postman’s User-Agent
                .build();

        try (Response response = client.newCall(request).execute()) {
            if (!response.isSuccessful()) throw new IOException("Unexpected code " + response);
            System.out.println("Response: " + response.body().string());
        }
    }
}
