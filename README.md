import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.util.HashMap;
import java.util.Map;

public class JWTRequestExample {
    public static void main(String[] args) {
        // Define the API endpoint
        String tokenUrl = "https://your-auth-service.com/token";

        // Define the username and password
        String username = "your-username";
        String password = "your-password";

        // Create an HTTP client
        HttpClient client = HttpClient.newHttpClient();

        // Build the request body
        Map<String, String> requestBody = new HashMap<>();
        requestBody.put("username", username);
        requestBody.put("password", password);
        String requestBodyJson = "{\"username\":\"" + username + "\", \"password\":\"" + password + "\"}";

        // Create the HTTP request
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create(tokenUrl))
                .header("Content-Type", "application/json")
                .POST(HttpRequest.BodyPublishers.ofString(requestBodyJson))
                .build();

        // Send the request
        HttpResponse<String> response;
        try {
            response = client.send(request, HttpResponse.BodyHandlers.ofString());
            String responseBody = response.body();

            // Parse the JWT token from the response
            // You may need to extract it from the JSON response.
            String jwtToken = responseBody;

            System.out.println("JWT Token: " + jwtToken);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
