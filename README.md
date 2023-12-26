import java.io.IOException;
import java.net.HttpURLConnection;
import java.net.URL;

public class UrlChecker {

    public static void main(String[] args) {
        String url = "http://example.com";

        try {
            int statusCode = checkUrlStatus(url);
            System.out.println("Status Code for " + url + ": " + statusCode);
        } catch (IOException e) {
            System.err.println("Error checking URL: " + e.getMessage());
        }
    }

    public static int checkUrlStatus(String urlString) throws IOException {
        URL url = new URL(urlString);
        HttpURLConnection connection = (HttpURLConnection) url.openConnection();

        try {
            connection.setRequestMethod("GET");

            // Set a timeout for the connection (in milliseconds)
            int timeout = 5000; // 5 seconds
            connection.setConnectTimeout(timeout);
            connection.setReadTimeout(timeout);

            // Connect to the URL
            connection.connect();

            // Get the HTTP response code
            return connection.getResponseCode();
        } finally {
            // Disconnect to release resources
            connection.disconnect();
        }
    }
}
