import okhttp3.*;
import java.io.IOException;
import java.util.Base64;

public class Main {
    public static void main(String[] args) throws IOException {
        // Initialize OkHttpClient
        OkHttpClient client = new OkHttpClient.Builder()
                .followRedirects(true)
                .followSslRedirects(true)
                .build();

        // API Details
        String apiUrl = "YOUR_API_ENDPOINT"; // Replace with actual API URL
        String clientId = "YOUR_CLIENT_ID";
        String clientSecret = "YOUR_CLIENT_SECRET";
        String grantType = "client_credentials";
        String resource = "YOUR_RESOURCE";

        // Encode Client ID & Secret for Basic Auth (if required)
        String credentials = clientId + ":" + clientSecret;
        String encodedCredentials = Base64.getEncoder().encodeToString(credentials.getBytes());

        // Define request body (x-www-form-urlencoded)
        MediaType mediaType = MediaType.parse("application/x-www-form-urlencoded");
        RequestBody body = RequestBody.create(mediaType, 
            "grant_type=" + grantType + "&resource=" + resource);

        // Build HTTP request
        Request request = new Request.Builder()
                .url(apiUrl)
                .post(body)
                .addHeader("Content-Type", "application/x-www-form-urlencoded")
                .addHeader("Authorization", "Basic " + encodedCredentials) // If API requires Basic Auth
                .addHeader("Accept", "application/json")
                .build();

        // Execute request
        Response response = client.newCall(request).execute();

        // Debugging: Print full response
        System.out.println("Response Code: " + response.code());
        System.out.println("Response Headers: " + response.headers());
        System.out.println("Response Body: " + response.body().string());
    }
}
