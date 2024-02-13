import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

import java.awt.Toolkit;

@SpringBootApplication
public class YourSpringApplication {

    public static void main(String[] args) {
        // Prevent the system from going to sleep
        keepSystemAwake();

        // Run the Spring application
        SpringApplication.run(YourSpringApplication.class, args);
    }

    private static void keepSystemAwake() {
        new Thread(() -> {
            while (true) {
                try {
                    // Keep the system awake for 1 minute
                    Thread.sleep(60000);
                    // Call Toolkit's beep to prevent the system from sleeping
                    Toolkit.getDefaultToolkit().beep();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }).start();
    }
}
