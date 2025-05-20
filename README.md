import java.io.FileInputStream;
import java.security.KeyStore;
import java.security.cert.X509Certificate;
import java.util.Date;
import java.util.Enumeration;

public class P12ExpiryChecker {
    public static void main(String[] args) {
        String p12FilePath = "path/to/yourfile.p12";
        String password = "your_password";

        try (FileInputStream fis = new FileInputStream(p12FilePath)) {
            // Load the keystore
            KeyStore keystore = KeyStore.getInstance("PKCS12");
            keystore.load(fis, password.toCharArray());

            // Iterate over all entries
            Enumeration<String> aliases = keystore.aliases();
            while (aliases.hasMoreElements()) {
                String alias = aliases.nextElement();
                X509Certificate cert = (X509Certificate) keystore.getCertificate(alias);

                if (cert != null) {
                    Date notAfter = cert.getNotAfter();
                    Date now = new Date();

                    System.out.println("Certificate alias: " + alias);
                    System.out.println("Expires on: " + notAfter);

                    if (now.after(notAfter)) {
                        System.out.println("❌ Certificate has EXPIRED.");
                    } else {
                        System.out.println("✅ Certificate is VALID.");
                    }
                }
            }

        } catch (Exception e) {
            e.printStackTrace();
            System.out.println("⚠️ Failed to read or process the .p12 file.");
        }
    }
}
