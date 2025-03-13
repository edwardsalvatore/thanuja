import okhttp3.*;

import java.io.IOException;

public class OAuthClientCredentials {
    public static void main(String[] args) throws IOException {
        OkHttpClient client = new OkHttpClient();

        // 🔹 Replace with your actual values
        String tokenUrl = "YOUR_AUTH_URL";   // Example: "https://login.microsoftonline.com/{tenant_id}/oauth2/token"
        String clientId = "YOUR_CLIENT_ID";
        String clientSecret = "YOUR_CLIENT_SECRET";
        String resource = "YOUR_RESOURCE";   // Sometimes required for Azure APIs
        String scope = "YOUR_SCOPE";         // Sometimes required (e.g., Okta, Google, Auth0)

        // 🔹 Construct form data (x-www-form-urlencoded)
        RequestBody body = new FormBody.Builder()
                .add("grant_type", "client_credentials")
                .add("client_id", clientId)
                .add("client_secret", clientSecret)
                .add("resource", resource)  // Remove if not needed
                .add("scope", scope)        // Remove if not needed
                .build();

        // 🔹 Build HTTP request
        Request request = new Request.Builder()
                .url(tokenUrl)
                .post(body)
                .addHeader("Content-Type", "application/x-www-form-urlencoded")
                .addHeader("Accept", "application/json")
                .addHeader("User-Agent", "PostmanRuntime/7.30.0")  // Matches Postman
                .build();

        // 🔹 Execute request
        Response response = client.newCall(request).execute();

        // 🔹 Print response
        System.out.println("Response Code: " + response.code());
        System.out.println("Response Body: " + response.body().string());
    }
}
