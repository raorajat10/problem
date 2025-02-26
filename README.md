import java.io.*;
import java.nio.file.*;
import java.nio.charset.StandardCharsets;

public class LogExtractor {
    public static void extractLogs(String logFile, String date) {
        String outputDir = "output";
        File dir = new File(outputDir);
        if (!dir.exists()) dir.mkdirs();
        
        String outputFile = outputDir + "/output_" + date + ".txt";
        
        try (BufferedReader reader = Files.newBufferedReader(Paths.get(logFile), StandardCharsets.UTF_8);
             BufferedWriter writer = Files.newBufferedWriter(Paths.get(outputFile), StandardCharsets.UTF_8)) {
            
            String line;
            while ((line = reader.readLine()) != null) {
                if (line.startsWith(date)) {
                    writer.write(line);
                    writer.newLine();
                }
            }
            System.out.println("Logs for " + date + " saved to " + outputFile);
        } catch (IOException e) {
            System.err.println("Error processing log file: " + e.getMessage());
        }
    }
    
    public static void main(String[] args) {
        if (args.length != 2) {
            System.out.println("Usage: java LogExtractor <log_file> <YYYY-MM-DD>");
            System.exit(1);
        }
        extractLogs(args[0], args[1]);
    }
}
