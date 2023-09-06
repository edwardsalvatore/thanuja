       try {
            DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
            DocumentBuilder builder = factory.newDocumentBuilder();
            InputSource inputSource = new InputSource(new StringReader(xmlData));

            // Parse the XML string
            Document document = builder.parse(inputSource);

            // Create a HashSet to store group-id-method pairs
            HashSet<String> groupIdMethodPairs = new HashSet<>();

            // Get all "group" elements
            NodeList groupList = document.getElementsByTagName("group");
            for (int i = 0; i < groupList.getLength(); i++) {
                Element groupElement = (Element) groupList.item(i);
                String groupId = groupElement.getAttribute("id");
                String method = null; // Initialize method as null

                // Check if the "domain" tag exists within the "group" element
                NodeList domainList = groupElement.getElementsByTagName("domain");
                if (domainList.getLength() > 0) {
                    Element domainElement = (Element) domainList.item(0);
                    method = domainElement.getAttribute("method");
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
        https://confluence.bnymellon.net/display/UAML3/How+to+raise+a+JIRA+ticket+to+access+AccessHub+Unix+servers
