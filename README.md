import java.io.*;
import java.net.*;
import java.nio.charset.StandardCharsets;
import java.util.stream.Collectors;
import org.json.JSONObject;

public class ApiClient {
    public static void main(String[] args) {
        try {
            // API & Authentication Details
            String TOKEN_URL = "https://login.microsoftonline.com/106bdeea-f616-4dfc-bc1d-6cbbf45e2011/oauth2/token";
            String CLIENT_ID = "b930264d-5a2-469d-ab9a-c1e868dc1f04";
            String CLIENT_SECRET = "g4m8Q-Ay0GYv1d15WbG7g5X9NFj4KbiF.6v6bajn";
            String RESOURCE = "https://bny-d36-uat.crm.dynamics.com/";
            String PROXY_HOST = "tpc-proxy.bnymellon.net";
            int PROXY_PORT = 8080;

            // Step 1: Get Access Token via Proxy
            String requestBody = "grant_type=client_credentials" +
                    "&client_id=" + CLIENT_ID +
                    "&client_secret=" + CLIENT_SECRET +
                    "&resource=" + RESOURCE;

            Proxy proxy = new Proxy(Proxy.Type.HTTP, new InetSocketAddress(PROXY_HOST, PROXY_PORT));
            URL tokenUrl = new URL(TOKEN_URL);
            HttpURLConnection tokenCon = (HttpURLConnection) tokenUrl.openConnection(proxy);
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

            // Step 2: Call the API Using the Token via Proxy
            String API_URL = "https://bny-d36-uat.crm.dynamics.com/api/data/v9.0/"; // Example API
            URL apiUrl = new URL(API_URL);
            HttpURLConnection apiCon = (HttpURLConnection) apiUrl.openConnection(proxy);
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
