import java.io.*;
import java.net.HttpURLConnection;
import java.net.URL;
import java.security.KeyStore;

import javax.net.ssl.HttpsURLConnection;
import javax.net.ssl.KeyManagerFactory;
import javax.net.ssl.SSLContext;
import javax.net.ssl.TrustManagerFactory;

public class TokenGenerator {
    public static void main(String[] args) throws Exception {
        // The API endpoint to generate the token
        String tokenUrl = "https://api.example.com/token";

        // Load the PKCS12 file
        String pkcs12FilePath = "path/to/client_certificate.p12";
        FileInputStream pkcs12FileInputStream = new FileInputStream(pkcs12FilePath);
        
        // Load the keystore
        KeyStore keyStore = KeyStore.getInstance("PKCS12");
        keyStore.load(pkcs12FileInputStream, "certificate_password".toCharArray());

        // Create SSL context with the keystore
        KeyManagerFactory keyManagerFactory = KeyManagerFactory.getInstance(KeyManagerFactory.getDefaultAlgorithm());
        keyManagerFactory.init(keyStore, "certificate_password".toCharArray());

        TrustManagerFactory trustManagerFactory = TrustManagerFactory.getInstance(TrustManagerFactory.getDefaultAlgorithm());
        trustManagerFactory.init(keyStore);

        SSLContext sslContext = SSLContext.getInstance("TLS");
        sslContext.init(keyManagerFactory.getKeyManagers(), trustManagerFactory.getTrustManagers(), null);

        // Open HTTPS connection with SSL context
        URL url = new URL(tokenUrl);
        HttpsURLConnection conn = (HttpsURLConnection) url.openConnection();
        conn.setSSLSocketFactory(sslContext.getSocketFactory());

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
            os.write("grant_type=client_cert".getBytes());
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
