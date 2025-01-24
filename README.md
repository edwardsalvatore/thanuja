import java.io.OutputStream;
import java.net.HttpURLConnection;
import java.net.InetSocketAddress;
import java.net.Proxy;
import java.net.URL;

public class ProxyRequest {
    public static void main(String[] args) {
        String url = "https://example.com/token"; // Replace with your URL
        String body = "grant_type=client_credentials&client_id=your_client_id&client_secret=your_client_secret";

        // Proxy server details (replace with your proxy details)
        String proxyHost = "proxy.example.com";
        int proxyPort = 8080;

        try {
            // Configure the proxy
            Proxy proxy = new Proxy(Proxy.Type.HTTP, new InetSocketAddress(proxyHost, proxyPort));
            HttpURLConnection connection = (HttpURLConnection) new URL(url).openConnection(proxy);

            // Set request method and headers
            connection.setRequestMethod("POST");
            connection.setRequestProperty("Content-Type", "application/x-www-form-urlencoded");
            connection.setRequestProperty("Accept", "*/*");
            connection.setDoOutput(true);

            // Write the request body
            try (OutputStream os = connection.getOutputStream()) {
                os.write(body.getBytes("UTF-8"));
            }

            // Get response code and print response
            int responseCode = connection.getResponseCode();
            System.out.println("Response Code: " + responseCode);

            // Handle response or redirect logic as needed

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
