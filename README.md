import org.apache.http.HttpEntity;
import org.apache.http.HttpHost;
import org.apache.http.HttpResponse;
import org.apache.http.auth.AuthScope;
import org.apache.http.auth.UsernamePasswordCredentials;
import org.apache.http.client.CredentialsProvider;
import org.apache.http.client.config.RequestConfig;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.BasicCredentialsProvider;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;
import java.io.IOException;

public class ApacheHttpClientWithProxyAuth {
    public static void main(String[] args) {
        String url = "https://login.microsoftonline.com/106bdeea-f616-4dfc-bc1d-6cbbf45e2011/oauth2/token";

        String requestBodyString =
                "client_id=b930264d-c5a2-469d-ab9a-c1e868dc1f04" +
                "&client_secret=g4m8Q-AyOGYvd1i5WbG7g5X9NFi4KBIf.6vGbajn" +
                "&grant_type=client_credentials" +
                "&resource=https://bny-d36-uat.crm.dynamics.com/" +
                "&tenantId=d1783452-59dc-4cf5-81f0-3d4e31b151cb";

        // Define Proxy
        HttpHost proxy = new HttpHost("tpc-proxy.bnymellon.net", 8080, "http");

        // Provide empty credentials for anonymous authentication
        CredentialsProvider credsProvider = new BasicCredentialsProvider();
        credsProvider.setCredentials(new AuthScope(proxy), new UsernamePasswordCredentials("", "")); // Empty creds

        RequestConfig config = RequestConfig.custom()
                .setProxy(proxy)
                .build();

        try (CloseableHttpClient client = HttpClients.custom()
                .setDefaultCredentialsProvider(credsProvider)
                .setDefaultRequestConfig(config)
                .build()) {

            // First request (Token request)
            HttpPost post = new HttpPost(url);
            post.setConfig(config);
            post.setHeader("Content-Type", "application/x-www-form-urlencoded");
            post.setHeader("Accept", "application/json");
            post.setHeader("User-Agent", "Mozilla/5.0 (Windows NT 10.0; Win64; x64)");

            post.setEntity(new StringEntity(requestBodyString));

            // Execute request through the proxy
            HttpResponse response = client.execute(post);
            int statusCode = response.getStatusLine().getStatusCode();
            System.out.println("Response Code: " + statusCode);

            if (statusCode == 302) {
                // Get the redirected URL
                String redirectUrl = response.getFirstHeader("Location").getValue();
                System.out.println("Redirecting to: " + redirectUrl);

                // Now make a GET request to the redirected URL
                HttpGet getRequest = new HttpGet(redirectUrl);
                getRequest.setConfig(config);
                getRequest.setHeader("User-Agent", "Mozilla/5.0");

                HttpResponse redirectResponse = client.execute(getRequest);
                HttpEntity entity = redirectResponse.getEntity();
                String responseBody = (entity != null) ? EntityUtils.toString(entity) : "No Response";

                System.out.println("Final Response Code: " + redirectResponse.getStatusLine().getStatusCode());
                System.out.println("Final Response Body: " + responseBody);
            } else {
                // Print the response body if there's no redirect
                HttpEntity entity = response.getEntity();
                String responseString = (entity != null) ? EntityUtils.toString(entity) : "No Response";
                System.out.println("Response Body: " + responseString);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
