import org.apache.commons.httpclient.HttpClient;
import org.apache.commons.httpclient.HttpMethod;
import org.apache.commons.httpclient.methods.PostMethod;
import org.apache.commons.httpclient.methods.StringRequestEntity;

public class SailPointPatchRequest {
    public static void main(String[] args) {
        try {
            // Create HTTP Client
            HttpClient client = new HttpClient();
            
            // Define API Endpoint
            String url = "https://example.com/api/resource";  // Replace with your actual API URL
            
            // Create a POST method and override to PATCH
            PostMethod request = new PostMethod(url) {
                @Override
                public String getName() {
                    return "PATCH";  // Override to send PATCH request
                }
            };
            
            // Set Headers
            request.setRequestHeader("Content-Type", "application/json");
            request.setRequestHeader("Authorization", "Bearer YOUR_ACCESS_TOKEN"); // Replace with actual token if needed
            
            // Define JSON Payload
            String jsonBody = "{\"key\": \"value\"}";  // Replace with your actual JSON data
            request.setRequestEntity(new StringRequestEntity(jsonBody, "application/json", "UTF-8"));
            
            // Execute HTTP Request
            int statusCode = client.executeMethod(request);
            String response = request.getResponseBodyAsString();
            
            // Print Response
            System.out.println("Response Code: " + statusCode);
            System.out.println("Response Body: " + response);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
