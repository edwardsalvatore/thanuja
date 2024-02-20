import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.net.HttpURLConnection;
import java.net.URL;

public class TokenGenerator {
    public static void main(String[] args) throws Exception {
        // The API endpoint to generate the token
        String tokenUrl = "https://api.example.com/token";

        // Request body for token generation
        String requestBody = "grant_type=client_cert";

        // Create URL object
        URL url = new URL(tokenUrl);
        
        // Open HTTP connection
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        
        // Set request method
        conn.setRequestMethod("POST");
        
        // Set request headers
        conn.setRequestProperty("Content-Type", "application/x-www-form-urlencoded");
        conn.setRequestProperty("Authorization", getBasicAuthHeader("username", "password"));
        
        // Enable input and output streams
        conn.setDoOutput(true);
        conn.setDoInput(true);

        // Write request body
        try (OutputStream os = conn.getOutputStream()) {
            os.write(requestBody.getBytes());
            os.flush();
        }

        // Read response
        try (BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream()))) {
            StringBuilder response = new StringBuilder();
            String line;
            while ((line = br.readLine()) != null) {
                response.append(line);
            }
            System.out.println("Token Response: " + response.toString());
        }

        // Close the connection
        conn.disconnect();
    }

    private static String getBasicAuthHeader(String username, String password) {
        String credentials = username + ":" + password;
        byte[] credentialsBytes = credentials.getBytes();
        return "Basic " + java.util.Base64.getEncoder().encodeToString(credentialsBytes);
    }
}
