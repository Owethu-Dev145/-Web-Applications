
# -Web-Applications


package com.mycompany.aidiary;

import java.util.Scanner;
import java.net.HttpURLConnection;
import java.net.URL;
import java.io.*;

public class AIDiary {
    
    static String API_KEY = "YOUR_API_KEY_HERE";
    
    public static void main(String[] args) throws Exception {
        Scanner scanner = new Scanner(System.in);
        
        System.out.println("============================");
        System.out.println("   Welcome to Your AI Diary ");
        System.out.println("============================");
        System.out.println("How was your day today?");
        System.out.print("You: ");
        
        String userInput = scanner.nextLine();
        
        System.out.println("\nAI is thinking...\n");
        
        String response = askAI(userInput);
        
        System.out.println("AI: " + response);
    }
    
    public static String askAI(String userMessage) throws Exception {
        URL url = new URL("https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=" + API_KEY);
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setRequestMethod("POST");
        conn.setRequestProperty("Content-Type", "application/json");
        conn.setDoOutput(true);
        
        String body = "{\"contents\":[{\"parts\":[{\"text\":\"You are a friendly diary assistant. The user says: " + userMessage + ". Ask them a thoughtful follow up question about their day.\"}]}]}";
        
        OutputStream os = conn.getOutputStream();
        os.write(body.getBytes());
        os.flush();
        
        BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream()));
        StringBuilder response = new StringBuilder();
        String line;
        while ((line = br.readLine()) != null) {
            response.append(line);
        }
        
        String raw = response.toString();
        int start = raw.indexOf("\"text\": \"") + 9;
        int end = raw.indexOf("\"", start);
        return raw.substring(start, end);
    }
}





 


 
