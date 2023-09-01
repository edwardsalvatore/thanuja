import org.w3c.dom.*;
import org.xml.sax.InputSource;
import javax.xml.parsers.*;
import java.io.StringReader;
import java.util.*;

public class XmlParser {
    public static void main(String[] args) {
        String xmlData = "<groups>" +
                "<group id='1' name='a'>" +
                "<constant name='z'/>" +
                "<domain id='1a' method='zebra' />" +
                "</group>" +
                "<group id='2' name='b'>" +
                "<constant name='y'/>" +
                "<domain id='1b' method='zea' />" +
                "</group>" +
                "<group id='3' name='a'>" +
                "<constant name='z'/>" +
                "<domain id='1c' />" +
                "</group>" +
                "</groups>";

        try {
            DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
            DocumentBuilder builder = factory.newDocumentBuilder();
            InputSource inputSource = new InputSource(new StringReader(xmlData));

            // Parse the XML string
            Document document = builder.parse(inputSource);

            // Create a HashSet to store id-method pairs
            HashSet<String> idMethodPairs = new HashSet<>();

            // Get all "domain" elements
            NodeList domainList = document.getElementsByTagName("domain");
            for (int i = 0; i < domainList.getLength(); i++) {
                Element domainElement = (Element) domainList.item(i);
                String id = domainElement.getAttribute("id");
                String method = domainElement.getAttribute("method");

                // If method is missing, add it as null
                if (method.isEmpty()) {
                    method = null;
                }

                // Add id-method pair to the HashSet
                idMethodPairs.add("id=" + id + ", method=" + method);
            }

            // Print the HashSet
            for (String pair : idMethodPairs) {
                System.out.println(pair);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
