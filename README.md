import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.config.RequestConfig;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;

import java.io.IOException;

public class ApacheHttpClientRedirectHandling {
    public static void main(String[] args) {
        String url = "https://login.microsoftonline.com/106bdeea-f616-4dfc-bc1d-6cbbf45e2011/oauth2/token";

        // Postman request body
        String requestBodyString =
                "client_id=b930264d-c5a2-469d-ab9a-c1e868dc1f04" +
                "&client_secret=g4m8Q-AyOGYvd1i5WbG7g5X9NFi4KBIf.6vGbajn" +
                "&grant_type=client_credentials" +
                "&resource=https://bny-d36-uat.crm.dynamics.com/" +
                "&tenantId=d1783452-59dc-4cf5-81f0-3d4e31b151cb";

        // Disable automatic redirects
        RequestConfig requestConfig = RequestConfig.custom()
                .setRedirectsEnabled(false)
                .build();

        try (CloseableHttpClient client = HttpClients.custom()
                .setDefaultRequestConfig(requestConfig)
                .build()) {

            HttpPost post = new HttpPost(url);

            // Set headers exactly as in Postman
            post.setHeader("Content-Type", "application/x-www-form-urlencoded");
            post.setHeader("Accept", "application/json");
            post.setHeader("User-Agent", "Mozilla/5.0 (Windows NT 10.0; Win64; x64)");

            // Set body
            post.setEntity(new StringEntity(requestBodyString));

            // Execute request
            HttpResponse response = client.execute(post);
            int statusCode = response.getStatusLine().getStatusCode();
            HttpEntity entity = response.getEntity();
            String responseString = (entity != null) ? EntityUtils.toString(entity) : "No Response";

            System.out.println("Response Code: " + statusCode);

            if (statusCode == 302) {
                // Get redirect location
                String redirectUrl = response.getFirstHeader("Location").getValue();
                System.out.println("Redirecting to: " + redirectUrl);
            }

            System.out.println("Response Body: " + responseString);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
