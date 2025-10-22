import org.apache.poi.ss.usermodel.*;
import org.apache.poi.xssf.usermodel.XSSFWorkbook;
 
import java.io.*;
import java.text.SimpleDateFormat;
import java.util.*;
 
public class ExcelSplitter {
 
    public static void main(String[] args) throws Exception {
        String excelFilePath = "requestdump.xlsx"; // Your Excel file
        FileInputStream fis = new FileInputStream(excelFilePath);
        Workbook workbook = new XSSFWorkbook(fis);
        Sheet sheet = workbook.getSheetAt(0);
 
        int dateColumnIndex = 6; // 7th column (0-based index)
 
        // Map to hold data grouped by month
        Map<String, List<List<String>>> monthlyData = new HashMap<>();
 
        // Prepare date formatter
        SimpleDateFormat inputFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        SimpleDateFormat keyFormat = new SimpleDateFormat("yyyyMM");
 
        // Read header row
        Row headerRow = sheet.getRow(0);
 
        for (int i = 1; i <= sheet.getLastRowNum(); i++) {
            Row row = sheet.getRow(i);
            if (row == null) continue;
 
            Cell dateCell = row.getCell(dateColumnIndex);
            if (dateCell == null) continue;
 
            dateCell.setCellType(CellType.STRING);
            String dateStr = dateCell.getStringCellValue();
 
            Date parsedDate;
            try {
                parsedDate = inputFormat.parse(dateStr);
            } catch (Exception e) {
                System.out.println("Skipping invalid date at row " + i + ": " + dateStr);
                continue;
            }
 
            String key = keyFormat.format(parsedDate); // e.g., "202203"
            monthlyData.putIfAbsent(key, new ArrayList<>());
 
            List<String> rowData = new ArrayList<>();
            for (Cell cell : row) {
                cell.setCellType(CellType.STRING);
                rowData.add(cell.getStringCellValue());
            }
 
            monthlyData.get(key).add(rowData);
        }
 
        // Prepare for writing
        SimpleDateFormat monthFormat = new SimpleDateFormat("MMM", Locale.ENGLISH);
 
        for (Map.Entry<String, List<List<String>>> entry : monthlyData.entrySet()) {
            String key = entry.getKey(); // e.g., 202203
            int year = Integer.parseInt(key.substring(0, 4));
            int month = Integer.parseInt(key.substring(4)) - 1;
 
            Calendar cal = Calendar.getInstance();
            cal.set(Calendar.YEAR, year);
            cal.set(Calendar.MONTH, month);
 
            String fileName = String.format("REQUESTDUMP_%s%04d.csv",
                    monthFormat.format(cal.getTime()).toUpperCase(), year);
 
            try (PrintWriter writer = new PrintWriter(new FileWriter(fileName))) {
                // Write header
                List<String> headers = new ArrayList<>();
                for (Cell cell : headerRow) {
                    headers.add(cell.getStringCellValue());
                }
                writer.println(String.join(",", headers));
 
                // Write rows
                for (List<String> rowData : entry.getValue()) {
                    writer.println(String.join(",", rowData));
                }
            }
        }
 
        workbook.close();
        fis.close();
 
        System.out.println("✅ Done! CSV files created for each month.");
    }
}
 
 
<dependencies>
    <dependency>
        <groupId>org.apache.poi</groupId>
        <artifactId>poi-ooxml</artifactId>
        <version>5.2.3</version>
    </dependency>
</dependencies>
