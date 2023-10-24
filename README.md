import javafx.animation.KeyFrame;
import javafx.animation.Timeline;
import javafx.application.Application;
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
            primaryStage.hide();  // Hide the window
            Duration delay = Duration.seconds(4);
            KeyFrame keyFrame = new KeyFrame(delay, e -> {
                primaryStage.show();  // Show the window again after 4 seconds
            });

            Timeline timeline = new Timeline(keyFrame);
            timeline.setCycleCount(1);  // Run once

            // Play the timeline to show the window again after 4 seconds
            timeline.play();
        });

        primaryStage.show();
    }
}
