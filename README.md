<!DOCTYPE html>
<html>
<head>
    <title>Input Form</title>
</head>
<body>
    <h1>Enter Values</h1>
    <form action="ProcessFormData" method="post">
        <div>
            <label for="x">x:</label>
            <input type="text" id="x" name="x" required>
        </div>
        <div>
            <label for="y">y:</label>
            <input type="text" id="y" name="y" required>
        </div>
        <div>
            <label for="z">z:</label>
            <input type="text" id="z" name="z" required>
        </div>
        <!-- You can add more fields here if needed -->
        <input type="submit" value="Submit">
    </form>
</body>
</html>import javax.servlet.*;
import javax.servlet.http.*;
import java.io.*;
import java.util.*;

public class ProcessFormData extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // Retrieve values from the HTML form
        String x = request.getParameter("x");
        String y = request.getParameter("y");
        String z = request.getParameter("z");
        
        // You can process the values here, e.g., add them to a map
        Map<String, String> map = new HashMap<>();
        map.put("1", x + "," + y + "," + z);

        // Send a response back (you can customize this)
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();
        out.println("<html><body>");
        out.println("<h1>Form Data Received</h1>");
        out.println("<p>x: " + x + "</p>");
        out.println("<p>y: " + y + "</p>");
        out.println("<p>z: " + z + "</p>");
        out.println("</body></html>");
    }
}
<dependencies>
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>4.0.1</version>
        <scope>provided</scope>
    </dependency>
</dependencies>

