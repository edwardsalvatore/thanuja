import okhttp3.*;

import java.io.IOException;

public class Main {
    public static void main(String[] args) throws IOException {
        OkHttpClient client = new OkHttpClient();

        MediaType mediaType = MediaType.parse("application/x-www-form-urlencoded");
        RequestBody body = RequestBody.create(mediaType, 
            "client_id=YOUR_CLIENT_ID&" +
            "client_secret=YOUR_CLIENT_SECRET&" +
            "grant_type=client_credentials&" +
            "resource=YOUR_RESOURCE");

        Request request = new Request.Builder()
            .url("YOUR_API_ENDPOINT") // Replace with your API URL
            .post(body)
            .addHeader("Content-Type", "application/x-www-form-urlencoded")
            .build();

        Response response = client.newCall(request).execute();

        if (response.isSuccessful()) {
            System.out.println("Response: " + response.body().string());
        } else {
            System.out.println("Error: " + response.code() + " - " + response.message());
        }
    }
}
