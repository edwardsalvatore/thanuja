https://central.sonatype.com/artifact/org.apache.poi/poi-ooxml/4.0.0?smo=true

import java.awt.*;
import java.awt.image.BufferedImage;
import java.io.*;
import java.net.URI;
import java.net.URISyntaxException;

import javax.imageio.ImageIO;

public class ScreenshotAndWord {
    public static void main(String[] args) throws URISyntaxException {
        String[] urls = { "https://www.example.com", "https://www.google.com", "https://www.github.com" };

        try {
            Robot robot = new Robot();
            Dimension screenSize = Toolkit.getDefaultToolkit().getScreenSize();
            Desktop desktop = Desktop.getDesktop();
            StringBuilder documentContent = new StringBuilder();
            for (int i = 0; i < urls.length; i++) {
                desktop.browse(new URI(urls[i]));

                // Wait for a moment for the page to load
                Thread.sleep(5000); // Adjust the delay as needed
                // Capture screenshot
                // Capture screenshot

                BufferedImage screenshot = robot
                        .createScreenCapture(new Rectangle(Toolkit.getDefaultToolkit().getScreenSize()));
                ImageIO.write(screenshot, "png", new File("screenshot" + i + ".png"));
                // Capture screenshot

                // Add a placeholder to the document
                documentContent.append("screenshot ").append(i + 1).append("\n");
                documentContent.append("![screenshot").append(i + 1).append("]\n");
            }

            // Save the text document
            try (FileWriter fileWriter = new FileWriter("screenshots.docx")) {
                fileWriter.write(documentContent.toString());
            }

            System.out.println("Text document with screenshot placeholders created successfully.");
        } catch (AWTException | IOException | InterruptedException e) {
            e.printStackTrace();
        }
    }
}
//////////////////////////////////
import java.awt.image.BufferedImage;
import java.awt.*;
import javax.imageio.ImageIO;
import java.io.*;
import java.util.ArrayList;

public class jj {
    public static void main(String[] args) {
        ArrayList<String> imageFiles = new ArrayList<>();
        for (int i = 1; i <= 10; i++) {
            imageFiles.add("s" + i + ".png");
        }

        try {
            XWPFDocument document = new XWPFDocument();

            for (String imageFile : imageFiles) {
                // Load image
                BufferedImage screenshot = ImageIO.read(new File(imageFile));

                // Add the image to the Word document
                XWPFParagraph paragraph = document.createParagraph();
                XWPFRun run = paragraph.createRun();
                run.addPicture(new FileInputStream(imageFile), XWPFDocument.PICTURE_TYPE_PNG, imageFile, Units.toEMU(screenshot.getWidth()), Units.toEMU(screenshot.getHeight()));
            }

            // Save the Word document
            FileOutputStream outputStream = new FileOutputStream("screenshots.docx");
            document.write(outputStream);
            outputStream.close();

            System.out.println("Word document with screenshots created successfully.");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

