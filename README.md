import java.sql.*;
import java.io.*;
import java.util.*;

public class MultiQueryExporter {

    public static void main(String[] args) {
        // --- Database connection setup ---
        String url = "jdbc:mysql://localhost:3306/your_database";  // change DB + port if needed
        String user = "root";
        String password = "your_password";

        // --- Queries and file names ---
        Map<String, String> queries = new LinkedHashMap<>();
        queries.put("sales_report.csv", "SELECT * FROM sales LIMIT 10");
        queries.put("customer_list.csv", "SELECT * FROM customers WHERE active=1");
        queries.put("product_stock.csv", "SELECT * FROM products WHERE quantity < 10");

        try (Connection conn = DriverManager.getConnection(url, user, password)) {
            System.out.println("✅ Database connected.");

            for (Map.Entry<String, String> entry : queries.entrySet()) {
                String fileName = entry.getKey();
                String query = entry.getValue();

                System.out.println("\n▶ Running query for: " + fileName);
                executeQueryAndSave(conn, query, fileName);
            }

        } catch (SQLException e) {
            System.out.println("❌ Database error: " + e.getMessage());
        }
    }

    // --- Method to run a query and save result as CSV ---
    private static void executeQueryAndSave(Connection conn, String query, String fileName) {
        try (Statement stmt = conn.createStatement();
             ResultSet rs = stmt.executeQuery(query)) {

            ResultSetMetaData meta = rs.getMetaData();
            int columnCount = meta.getColumnCount();

            if (!rs.isBeforeFirst()) { // check if ResultSet is empty
                System.out.println("⚠️ Data not found for " + fileName);
                return;
            }

            try (PrintWriter writer = new PrintWriter(new File(fileName))) {

                // --- Write header ---
                for (int i = 1; i <= columnCount; i++) {
                    writer.print(meta.getColumnName(i));
                    if (i < columnCount) writer.print(",");
                }
                writer.println();

                // --- Write rows ---
                while (rs.next()) {
                    for (int i = 1; i <= columnCount; i++) {
                        writer.print(rs.getString(i));
                        if (i < columnCount) writer.print(",");
                    }
                    writer.println();
                }
            }

            System.out.println("✅ File created: " + fileName);

        } catch (SQLException | IOException e) {
            System.out.println("❌ Error running query or writing file: " + e.getMessage());
        }
    }
}
