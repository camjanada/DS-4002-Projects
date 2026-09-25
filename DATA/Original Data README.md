Our original Rotten Tomatoes data was too big to upload onto GitHub as it was 155MB and GitHub only accepts filed up to 25MB. 
When we downloaded the data from Kaggle, it was a zip folder that, when unzipped, would create a csv. With that csv, we has to use
Power Query first to ensure the scores which were often wrote like 8/10 didnt become written dates when transformed into an excel.
Once it was an excel, we filtered by date and deleted duplicates and unusable rows like blank original scores and review texts to 
ensure all data we kept was usable and after COVID. After that, we cleaned and standardized the scores from 0-100.
