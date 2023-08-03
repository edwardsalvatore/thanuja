 public static String findValue(String xmlData,String findValue) throws ParserConfigurationException, SAXException, IOException{
        DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
            DocumentBuilder builder = factory.newDocumentBuilder();

            // Parse the XML string into a Document object
            Document document = builder.parse(new ByteArrayInputStream(xmlData.getBytes()));

            // Get a list of all elements with the attribute "token"
            NodeList nodeList = document.getElementsByTagName("*");
            for (int i = 0; i < nodeList.getLength(); i++) {
                Element element = (Element) nodeList.item(i);
                String tokenValue = element.getAttribute("token");
                if (!tokenValue.isEmpty()) {
                    System.out.println("Token value: " + tokenValue);
                }
                return tokenValue;
            }
            return findValue;
            
    }
