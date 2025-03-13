import okhttp3.*;
import java.io.IOException;
import java.util.Base64;

public class Main {
    public static void main(String[] args) throws IOException {
        OkHttpClient client = new OkHttpClient();

        // Encode client_id and client_secret in Base64 (if required)
        String credentials = "YOUR_CLIENT_ID:YOUR_CLIENT_SECRET";
        String encodedCredentials = Base64.getEncoder().encodeToString(credentials.getBytes());

        MediaType mediaType = MediaType.parse("application/x-www-form-urlencoded");
        RequestBody body = RequestBody.create(mediaType,
                "grant_type=client_credentials&resource=YOUR_RESOURCE");

        Request request = new Request.Builder()
                .url("YOUR_API_ENDPOINT") // Replace with the actual API endpoint
                .post(body)
                .addHeader("Content-Type", "application/x-www-form-urlencoded")
                .addHeader("Authorization", "Basic " + encodedCredentials) // Add Authorization header
                .build();

        Response response = client.newCall(request).execute();

        if (response.isSuccessful()) {
            System.out.println("Response: " + response.body().string());
        } else {
            System.out.println("Error: " + response.code() + " - " + response.message());
            System.out.println("Response Body: " + response.body().string()); // Print error details
        }
    }
}
