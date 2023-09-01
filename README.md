

        try {
            DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
            DocumentBuilder builder = factory.newDocumentBuilder();
            InputSource inputSource = new InputSource(new StringReader(xmlData));

            // Parse the XML string
            Document document = builder.parse(inputSource);

            // Create a HashSet to store group-id-method pairs
            HashSet<String> groupIdMethodPairs = new HashSet<>();

            // Get all "domain" elements
            NodeList domainList = document.getElementsByTagName("domain");
            for (int i = 0; i < domainList.getLength(); i++) {
                Element domainElement = (Element) domainList.item(i);
                String groupId = domainElement.getParentNode().getAttributes().getNamedItem("id").getNodeValue();
                String method = domainElement.getAttribute("method");

                // If method is missing, add it as null
                if (method.isEmpty()) {
                    method = null;
                }

                // Add group-id-method pair to the HashSet
                groupIdMethodPairs.add("group-id=" + groupId + ", method=" + method);
            }

            // Print the HashSet
            for (String pair : groupIdMethodPairs) {
                System.out.println(pair);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }

