# Play store apps Data analysis with SQL

![](image_intro.png)

## Quick overview
Welcome to the Play Store Analysis project! This repository showcases a comprehensive analysis of app data from the Google Play Store using SQL. By leveraging a rich dataset, the project aims to uncover insights related to app performance, user ratings, and versions in various categories. All key feautures and research questions were 



## Key features of this analysis include
- Data Exploration: Initial exploration of app attributes, including ratings, downloads, and categories.
- Trends and Patterns: Identification of trends in user preferences and app popularity over time.
- Comparative Analysis: Comparison of top-performing apps to understand what drives success in the Play Store.

  ## Research questions
  1. which is the app has the highest number of installation?
  2. which are the top 10 apps with lowest number on installation?
  3. what are the most installed app in various categories?
  4. What are the average ratings of apps by category?
  5. What are the top-rated apps in each category?
  6. What is the correlation between app size, app version, and user ratings on the Google Play Store?
  7. Does the app version affect the number of installations?
  8. What are the most popular app categories ?
  9. What are the most common app versions on the Google Play Store?

 ## Goals this project aims to achieve
 
 The primary goal of this research project is to analyze app data from the Google Play Store to uncover trends and insights that inform developers and stakeholders about app performance and user preferences. By addressing the research questions, we aim to
 
- Identify key factors influencing app installations and ratings.
- Understand the dynamics of app popularity across different categories.
- Assess the impact of app size, version, and other characteristics on user satisfaction and engagement.
- Provide actionable insights that can guide future app development and marketing strategies.
    
Ultimately, this analysis seeks to enhance the understanding of the mobile app landscape, benefiting both developers and users.

 ### Disclaimer: 
 The data presented in this analysis is based on research and publicly available information from the Google Play Store. The datasets used are not curated by me and may contain inaccuracies or discrepancies. The insights drawn from this data are intended for informational purposes only and should be interpreted with caution.

 ## SQL query and Ouput

 1.which is the app has the highest number of installation?
 This question aims to identify the app with the greatest number of installations, providing insights into its popularity and user acceptance.
 
     SELECT App, MAX(Installs) as Most_Installed
     FROM 'Google_playstore_apps.csv'
     GROUP BY App
     ORDER BY Most_Installed DESC
     LIMIT 1;

  Output
  
  ![](result1)

  
      
