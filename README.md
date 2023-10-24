import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.TextField;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

import java.util.ArrayList;

public class JavaFXExample extends Application {
    private ArrayList<String> textList = new ArrayList<>();
    private TextField inputField;
    private TextField displayField;

    public static void main(String[] args) {
        launch(args);
    }

    @Override
    public void start(Stage primaryStage) {
        primaryStage.setTitle("Input Text and Add to ArrayList");
        
        inputField = createInputField();
        Button addButton = createAddButton();
        displayField = createDisplayField();

        addButton.setOnAction(e -> handleAddButtonClick());

        VBox vBox = new VBox(inputField, addButton, displayField);
        vBox.setSpacing(10);
        Scene scene = new Scene(vBox, 300, 200);
        primaryStage.setScene(scene);

        primaryStage.show();
    }

    private TextField createInputField() {
        TextField textField = new TextField();
        textField.setPromptText("Enter text");
        return textField;
    }

    private Button createAddButton() {
        Button button = new Button("Add to List");
        return button;
    }

    private TextField createDisplayField() {
        TextField textField = new TextField();
        textField.setPromptText("Added Items");
        return textField;
    }

    private void handleAddButtonClick() {
        String text = inputField.getText();
        if (!text.isEmpty()) {
            textList.add(text);
            inputField.clear();
            displayField.setText(textList.toString());
        }
    }
}
