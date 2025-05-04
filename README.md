import javafx.animation.PauseTransition;
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.layout.*;
import javafx.stage.Stage;
import javafx.util.Duration;

import java.awt.*;
import java.awt.image.BufferedImage;
import java.io.*;
import java.nio.file.*;
import java.util.ArrayList;
import javax.imageio.ImageIO;

import org.apache.poi.xwpf.usermodel.*;

public class HideAndShow extends Application {

    int p = 0, c = 0;
    ArrayList<String> textList = new ArrayList<>();
    private TextField inputField;
    private TextField displayField;
    private TextField inputDocName;
    private String docname = null;
    Button generateDocumentButton = new Button("Generate Documentation");

    public static void main(String[] args) {
        launch(args);
    }

    @Override
    public void start(Stage primaryStage) {
        primaryStage.setTitle("Screenshot and Documentation App");

        Button captureScreenshotButton = new Button("Capture Screenshot");
        StackPane sp = new StackPane();
        sp.getChildren().add(captureScreenshotButton);

        inputField = createInputField();
        Button addButton = createAddButton();
        displayField = createDisplayField();
        inputDocName = createInputFieldforDoc();

        generateDocumentButton.setOnAction(e -> {
            try {
                generateDocumentation();
            } catch (IOException ex) {
                throw new RuntimeException(ex);
            }
        });

        addButton.setOnAction(e -> handleAddButtonClick());

        captureScreenshotButton.setOnAction(e -> {
            p++;

            primaryStage.hide(); // Hide the frame first

            PauseTransition pause = new PauseTransition(Duration.millis(500));
            pause.setOnFinished(event -> {
                try {
                    System.out.println("Taking screenshot...");
                    captureScreenshots(p);
                } catch (AWTException | IOException ex) {
                    throw new RuntimeException(ex);
                } finally {
                    primaryStage.show(); // Show it back after screenshot
                }
            });
            pause.play();
        });

        VBox vbox = new VBox(captureScreenshotButton,
                generateDocumentButton, addButton, inputField, inputDocName);
        Scene scene = new Scene(vbox, 300, 200);
        primaryStage.setScene(scene);
        primaryStage.show();
    }

    private TextField createInputField() {
        TextField textField = new TextField();
        textField.setPromptText("Enter text");
        return textField;
    }

    private TextField createInputFieldforDoc() {
        TextField textField = new TextField();
        textField.setPromptText("Enter Document Name");
        return textField;
    }

    private Button createAddButton() {
        return new Button("Add to Document");
    }

    private TextField createDisplayField() {
        TextField textField = new TextField();
        textField.setPromptText("Added Items");
        return textField;
    }

    private String handleAddButtonDocClick() {
        String text = inputDocName.getText();
        if (!text.isEmpty()) {
            docname = text;
        }
        return docname;
    }

    private void handleAddButtonClick() {
        String text = inputField.getText();
        if (!text.isEmpty()) {
            textList.add(text);
            p++;
            inputField.clear();
        }
    }

    private void captureScreenshots(int p) throws AWTException, IOException {
        Robot robot = new Robot();
        BufferedImage screenshot = robot.createScreenCapture(
                new Rectangle(Toolkit.getDefaultToolkit().getScreenSize()));
        ImageIO.write(screenshot, "png", new File("s" + p + ".png"));
    }

    private void generateDocumentation() throws IOException {
        ArrayList<String> imageFiles = new ArrayList<>();
        XWPFDocument document = new XWPFDocument();

        for (int j = 1; j <= p; j++) {
            try {
                String imageFile = "s" + j + ".png";
                try {
                    BufferedImage screenshot = ImageIO.read(new File(imageFile));
                    XWPFParagraph imageParagraph = document.createParagraph();
                    XWPFRun imageRun = imageParagraph.createRun();
                    InputStream imageStream = Files.newInputStream(Paths.get(imageFile));

                    int width = screenshot.getWidth();
                    int height = screenshot.getHeight();
                    int newWidth = 400;
                    int newHeight = (int) ((double) newWidth / width * height);
                    imageRun.addPicture(imageStream, XWPFDocument.PICTURE_TYPE_PNG,
                            imageFile, Units.toEMU(newWidth), Units.toEMU(newHeight));
                    imageStream.close();
                } catch (Exception e) {
                    XWPFParagraph noteParagraph = document.createParagraph();
                    XWPFRun noteRun = noteParagraph.createRun();
                    noteRun.setText(textList.get(c));
                    c++;
                    continue;
                }
            } catch (Exception e) {
                System.out.println(e);
            }
        }

        String name = handleAddButtonDocClick();
        FileOutputStream outputStream = new FileOutputStream(name + ".docx");
        document.write(outputStream);
        outputStream.close();
        System.out.println("Word document with screenshots created successfully.");

        for (int i = 1; i <= p; i++) {
            String pht = "s" + i + ".png";
            File io = new File(pht);
            if (io.exists()) {
                io.delete();
            }
        }
    }
}
