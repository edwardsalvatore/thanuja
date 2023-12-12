import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;

public class SplunkIntegration {
    public static void main(String[] args) {
        try {
            // Replace with your Splunk API endpoint and search query
            String splunkEndpoint = "https://your-splunk-instance:8089/services/search/jobs/export";

            // Construct your search query with date and time criteria
            String searchQuery = "search index=_internal sourcetype=splunkd earliest=\"12/01/2023:00:00:00\" latest=\"12/01/2023:23:59:59\" | head 5";

            URL url = new URL(splunkEndpoint + "?q=" + searchQuery);
            HttpURLConnection connection = (HttpURLConnection) url.openConnection();

            // Set up authentication if needed
            connection.setRequestProperty("Authorization", "Basic " + base64Encode("your-username:your-password"));

            // Read the response
            BufferedReader reader = new BufferedReader(new InputStreamReader(connection.getInputStream()));
            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println(line);
            }
            reader.close();

            // Close the connection
            connection.disconnect();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    // Helper method to encode credentials
    private static String base64Encode(String input) {
        return java.util.Base64.getEncoder().encodeToString(input.getBytes());
    }
}
