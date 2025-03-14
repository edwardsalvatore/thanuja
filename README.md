import java.io.*;
import java.net.*;
import java.nio.charset.StandardCharsets;
import java.util.stream.Collectors;
import org.json.JSONObject;

public class ApiClient {
    public static void main(String[] args) {
        try {
            // Replace these with actual values
            String TOKEN_URL = "https://your-token-url"; // Token endpoint
            String CLIENT_ID = "your-client-id";
            String CLIENT_SECRET = "your-client-secret";
            String API_URL = "https://your-api-url"; // API to call
            boolean USE_PROXY = false; // Change to true if proxy is needed
            String PROXY_HOST = "your-proxy-host"; // Proxy hostname (if needed)
            int PROXY_PORT = 8080; // Proxy port (if needed)

            // Step 1: Get the OAuth2 Token
            String requestBody = "grant_type=client_credentials&client_id=" + CLIENT_ID +
                    "&client_secret=" + CLIENT_SECRET +
                    "&scope=your-api-scope"; // Add scope if required

            URL tokenUrl = new URL(TOKEN_URL);
            HttpURLConnection tokenCon = USE_PROXY
                    ? (HttpURLConnection) tokenUrl.openConnection(new Proxy(Proxy.Type.HTTP, new InetSocketAddress(PROXY_HOST, PROXY_PORT)))
                    : (HttpURLConnection) tokenUrl.openConnection();

            tokenCon.setRequestMethod("POST");
            tokenCon.setRequestProperty("Content-Type", "application/x-www-form-urlencoded");
            tokenCon.setDoOutput(true);

            try (OutputStream os = tokenCon.getOutputStream()) {
                os.write(requestBody.getBytes(StandardCharsets.UTF_8));
            }

            int tokenResponseCode = tokenCon.getResponseCode();
            if (tokenResponseCode != 200) {
                System.out.println("Failed to fetch token. Response Code: " + tokenResponseCode);
                return;
            }

            BufferedReader tokenReader = new BufferedReader(new InputStreamReader(tokenCon.getInputStream()));
            String tokenResponse = tokenReader.lines().collect(Collectors.joining());
            JSONObject tokenJson = new JSONObject(tokenResponse);
            String accessToken = tokenJson.getString("access_token");

            System.out.println("Access Token: " + accessToken);

            // Step 2: Call the API using the obtained token
            URL apiUrl = new URL(API_URL);
            HttpURLConnection apiCon = USE_PROXY
                    ? (HttpURLConnection) apiUrl.openConnection(new Proxy(Proxy.Type.HTTP, new InetSocketAddress(PROXY_HOST, PROXY_PORT)))
                    : (HttpURLConnection) apiUrl.openConnection();

            apiCon.setRequestMethod("GET");
            apiCon.setRequestProperty("Authorization", "Bearer " + accessToken);
            apiCon.setRequestProperty("Accept", "application/json");

            int apiResponseCode = apiCon.getResponseCode();
            if (apiResponseCode != 200) {
                System.out.println("API call failed. Response Code: " + apiResponseCode);
                return;
            }

            BufferedReader apiReader = new BufferedReader(new InputStreamReader(apiCon.getInputStream()));
            String apiResponse = apiReader.lines().collect(Collectors.joining());

            System.out.println("API Response: " + apiResponse);

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
