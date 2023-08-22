                XWPFParagraph noteParagraph = document.createParagraph();
                XWPFRun noteRun = noteParagraph.createRun();
                noteRun.setText("Note for Image " + imageFile); // Add your note here

                // Create a run and add picture
                XWPFParagraph imageParagraph = document.createParagraph();
                XWPFRun imageRun = imageParagraph.createRun();
                InputStream imageStream = new FileInputStream(imageFile);
                
                // Set the image dimensions
                int width = screenshot.getWidth();
                int height = screenshot.getHeight();
                int newWidth = 400; // Adjust as needed
                int newHeight = (int) ((double) newWidth / width * height);
                
                imageRun.addPicture(imageStream, XWPFDocument.PICTURE_TYPE_PNG, imageFile, Units.toEMU(newWidth), Units.toEMU(newHeight));
                imageStream.close();
