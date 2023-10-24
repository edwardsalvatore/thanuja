import javafx.animation.KeyFrame;
import javafx.animation.Timeline;
import javafx.application.Application;
import javafx.application.Platform;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.layout.StackPane;
import javafx.stage.Stage;
import javafx.util.Duration;

public class HideAndShowExample extends Application {

    public static void main(String[] args) {
        launch(args);
    }

    @Override
    public void start(Stage primaryStage) {
        primaryStage.setTitle("Hide and Show Example");

        Button button = new Button("Click Me");
        StackPane root = new StackPane();
        root.getChildren().add(button);

        Scene scene = new Scene(root, 300, 250);
        primaryStage.setScene(scene);

        button.setOnAction(event -> {
            // Create a Timeline to hide the window after 4 seconds
            Duration delay = Duration.seconds(4);
            KeyFrame keyFrame = new KeyFrame(delay, e -> {
                // Use Platform.runLater to execute this on the JavaFX application thread
                Platform.runLater(() -> {
                    primaryStage.hide();
                    // Show the window again after 4 seconds
                    Timeline showTimeline = new Timeline(new KeyFrame(Duration.seconds(4), evt -> primaryStage.show()));
                    showTimeline.play();
                });
            });

            Timeline hideTimeline = new Timeline(keyFrame);
            hideTimeline.setCycleCount(1);  // Run once

            // Play the timeline to hide the window after 4 seconds
            hideTimeline.play();
        });

        primaryStage.show();
    }
}
