import java.net.Authenticator;
import java.net.PasswordAuthentication;
import okhttp3.*;

public class NTLMAuthExample {
    public static void main(String[] args) throws Exception {
        OkHttpClient client = new OkHttpClient.Builder()
            .proxyAuthenticator(new ProxyAuthenticator("YOUR_USERNAME", "YOUR_PASSWORD"))
            .build();

        Request request = new Request.Builder()
            .url("YOUR_API_ENDPOINT") // Make sure this is the correct API URL
            .build();

        Response response = client.newCall(request).execute();
        System.out.println("Response: " + response.code() + " - " + response.body().string());
    }

    static class ProxyAuthenticator implements okhttp3.Authenticator {
        private final String username;
        private final String password;

        ProxyAuthenticator(String username, String password) {
            this.username = username;
            this.password = password;
        }

        @Override
        public Request authenticate(Route route, Response response) {
            String credential = Credentials.basic(username, password);
            return response.request().newBuilder()
                .header("Proxy-Authorization", credential)
                .build();
        }
    }
}
