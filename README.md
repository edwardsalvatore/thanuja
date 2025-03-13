import okhttp3.*;
import java.io.IOException;
import java.util.Base64;

public class Main {
    public static void main(String[] args) throws IOException {
        // Initialize OkHttpClient with redirect handling
        OkHttpClient client = new OkHttpClient.Builder()
                .followRedirects(true)
                .followSslRedirects(true)
                .build();

        // API Credentials
        String clientId = "YOUR_CLIENT_ID";
        String clientSecret = "YOUR_CLIENT_SECRET";
        String apiUrl = "YOUR_API_ENDPOINT";  // Replace with actual API endpoint

        // Encode Client ID & Secret in Base64 for Basic Auth (if needed)
        String credentials = clientId + ":" + clientSecret;
        String encodedCredentials = Base64.getEncoder().encodeToString(credentials.getBytes());

        // Define request body (x-www-form-urlencoded format)
        MediaType mediaType = MediaType.parse("application/x-www-form-urlencoded");
        RequestBody body = RequestBody.create(mediaType, 
            "grant_type=client_credentials&resource=YOUR_RESOURCE");

        // Build HTTP request
        Request request = new Request.Builder()
                .url(apiUrl)
                .post(body)
                .addHeader("Content-Type", "application/x-www-form-urlencoded")
                .addHeader("Authorization", "Basic " + encodedCredentials) // If API requires Basic Auth
                .addHeader("Accept", "application/json") // Ensure response is JSON
                .build();

        // Execute request
        Response response = client.newCall(request).execute();

        // Handle response
        if (response.isSuccessful()) {
            System.out.println("Response: " + response.body().string());
        } else {
            System.out.println("Error: " + response.code() + " - " + response.message());
            System.out.println("Response Body: " + response.body().string()); // Print error details
        }
    }
}
