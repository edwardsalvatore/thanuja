import java.awt.image.BufferedImage;
import java.awt.*;
import javax.imageio.ImageIO;
import java.io.*;
import java.util.ArrayList;
import org.apache.poi.xwpf.usermodel.*;

public class ScreenshotAndWord {
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

                // Create a run and add picture
                XWPFParagraph paragraph = document.createParagraph();
                XWPFRun run = paragraph.createRun();
                byte[] imageBytes = getImageBytes(imageFile);
                run.addPicture(new ByteArrayInputStream(imageBytes), XWPFDocument.PICTURE_TYPE_PNG, imageFile, Units.toEMU(screenshot.getWidth()), Units.toEMU(screenshot.getHeight()));
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

    private static byte[] getImageBytes(String imageFile) throws IOException {
        BufferedImage image = ImageIO.read(new File(imageFile));
        ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
        ImageIO.write(image, "png", outputStream);
        return outputStream.toByteArray();
    }
}
