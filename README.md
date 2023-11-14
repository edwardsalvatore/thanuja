Pattern pattern = Pattern.compile("value:(\\S+)");

        // Create a matcher with the input string
        Matcher matcher = pattern.matcher(input);

        // Find all matches
        while (matcher.find()) {
            // Extract and print the value
            String value = matcher.group(1);
            System.out.println(value);
        }
