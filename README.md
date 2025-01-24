try (BufferedReader reader = new BufferedReader(new InputStreamReader(connection.getInputStream()))) {
    StringBuilder response = new StringBuilder();
    String line;
    while ((line = reader.readLine()) != null) {
        response.append(line);
    }
    System.out.println("Response Body: " + response.toString());
}
