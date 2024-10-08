# Play store apps Data analysis with SQL

![](image_intro.png)

# Table of content
- Quick overview
- Key features
- Research questions
- Goals aimed to achieve
- Disclaimer
- SQL query and output
- Business Insights and conclusion

  

### Quick overview
Welcome to the Play Store Analysis project! This repository showcases a comprehensive analysis of app data from the Google Play Store using SQL. By leveraging a rich dataset, the project aims to uncover insights related to app performance, user ratings, and versions in various categories. All key feautures and research questions were developed by myself.



### Key features of this analysis include
- Data Exploration: Initial exploration of app attributes, including ratings, downloads, and categories.
- Trends and Patterns: Identification of trends in user preferences and app popularity over time.
- Comparative Analysis: Comparison of top-performing apps to understand what drives success in the Play Store.

### Research questions
  1. which is the app has the highest number of installation?
  2. which are the top 10 apps with lowest number on installation?
  3. what are the most installed app in various categories?
  4. What are the average ratings of apps by category?
  5. What are the top-rated apps in each category?
  6. What is the correlation between app size, app version, and user ratings on the Google Play Store?
  7. Does the app version affect the number of installations?
  8. What are the most popular app categories by reviews ?
  9. What are the most common app versions on the Google Play Store?
  10. What are the factores influencing top installed app and least installed app.

 ### Goals this project aims to achieve
 
 The primary goal of this research project is to analyze app data from the Google Play Store to uncover trends and insights that inform developers and stakeholders about app performance and user preferences. By addressing the research questions, we aim to:
 
- Identify key factors influencing app installations and ratings.
- Understand the dynamics of app popularity across different categories.
- Assess the impact of app size, version, and other characteristics on user satisfaction and engagement.
- Provide actionable insights that can guide future app development and marketing strategies.
    
Ultimately, this analysis seeks to enhance the understanding of the mobile app landscape, benefiting both developers and users.

 ### Disclaimer: 
 The data presented in this analysis is based on research and publicly available information from the Google Play Store. The datasets used are not curated by me and may contain inaccuracies or discrepancies. The insights drawn from this data are intended for informational purposes only and should be interpreted with caution.

 ### SQL query and Ouput

 1.which is the app has the highest number of installation?
 This question aims to identify the app with the greatest number of installations, providing insights into its popularity and user acceptance.
 
     SELECT App, MAX(Installs) as Most_Installed
     FROM 'Google_playstore_apps.csv'
     GROUP BY App
     ORDER BY Most_Installed DESC
     LIMIT 1

  Output
  
  ![](result1.png)
  
2.which are the top 10 apps with lowest number on installation?
     This question seeks to identify the apps with the lowest number of installations, providing insights into potential challenges they face in gaining user traction.

        SELECT App, MIN(Installs) as less_Installed
        FROM 'Google_playstore_apps.csv'
        GROUP BY App
        ORDER BY less_Installed 
        LIMIT 10

Output:

![](result2.png)

3.what are the most installed app in various categories?
     This question aims to identify the most installed app in various categories (e.g., games, productivity, social media) on the Google Play Store. Analyzing installation data by category allows us to understand trends in user preferences and behaviors across different types of applications.

       SELECT 
       Category, 
       COUNT(App) AS App_Count
       FROM 'Google_playstore_apps.csv'
       GROUP BY Category
       ORDER BY App_Count DESC

  Ouput:

  ![](result3.png)
  
  
4.what are the average ratings of apps by category?
     This question seeks to determine the average user ratings for apps within each category . By analyzing average ratings, we can gain insights into overall user satisfaction and quality perceptions across different types of applications.
     

        SELECT 
        Category, 
        AVG(Rating) AS Average_Rating
        FROM 'Google_playstore_apps.csv'
        GROUP BY Category
        ORDER BY Average_Rating DESC

Output:

![](result4.png)


5.What are the top-rated apps in each category?

     SELECT 
     Category, 
     App, 
     Rating
     FROM (
     SELECT 
        Category, 
        App, 
        Rating, 
        ROW_NUMBER() OVER (PARTITION BY Category ORDER BY Rating DESC) AS rank
     FROM 'Google_playstore_apps.csv'
     ) AS ratings
     WHERE rank = 1

   Output:

   ![](result5.png)
   

6.What is the correlation between app size, app version, and user ratings on the Google Play Store?

      SELECT 
      App,
      Rating,
      Size,
      Current_Ver AS version
      FROM 'Google_playstore_apps.csv'
      WHERE Rating < 3.0
      ORDER BY Rating ASC

   Output:

   ![](result6.png)
   


7.Does the app version affect the number of installations?

        SELECT 
        Current_Ver AS Version, 
        AVG(CAST(REPLACE(REPLACE(CAST(Installs AS VARCHAR), '+', ''), ',', '') AS INTEGER)) AS Average_Installs,
	      MAX(CAST(REPLACE(REPLACE(CAST(Installs AS VARCHAR), '+', ''), ',', '') AS INTEGER)) AS Maximum_Installs
        FROM 'Google_playstore_apps.csv'
        GROUP BY Current_Ver
        ORDER BY Average_Installs DESC
        
 Output:

 ![](result7.png)
 This query shows that the average number of installations for each app version, which will allow us to see if certain versions are associated with higher installations.

    
8.What are the most popular app categories by reviews ?

      SELECT 
      App,
      Category,
      Reviews
      FROM 'Google_playstore_apps.csv'
      ORDER BY Category, Reviews DESC
      LIMIT 10

  Ouput:

  ![](result8.png)

     
9.What are the most common app versions on the Google Play Store?

      SELECT 
      Current_Ver AS Version, 
      COUNT(App) AS App_Count
      FROM 'Google_playstore_apps.csv'
      GROUP BY Current_Ver
      ORDER BY App_Count DESC

Output:

![](result9.png)

10. What are the factors influencing top installed and least installed app
    Here, i ran a query that selects only the category, ratings, reviews, size, installs and current version for both dataset(top installed and least installed.
    From this result, we can gather insights into what makes some apps more installed than others.

Top installed app

        WITH TopInstalledApps AS (
        SELECT 
        *,
        CAST(REPLACE(REPLACE(CAST(Installs AS VARCHAR), '+', ''), ',', '') AS INTEGER) AS Installs_Cleaned
        FROM 'Google_playstore_apps.csv'
        WHERE CAST(REPLACE(REPLACE(CAST(Installs AS VARCHAR), '+', ''), ',', '') AS INTEGER) >= (
        SELECT PERCENTILE_CONT(0.90) WITHIN GROUP (ORDER BY CAST(REPLACE(REPLACE(CAST(Installs AS VARCHAR), '+', ''), ',', '') AS INTEGER))
        FROM 'Google_playstore_apps.csv'
        )
        )
        SELECT Category, Rating, Reviews, Size,Installs, Current_Ver
        FROM TopInstalledApps
        WHERE Rating > 3.6
        ORDER BY App DESC

Least installed app

      WITH LeastInstalledApps AS (
      SELECT 
        *,
        CAST(REPLACE(REPLACE(CAST(Installs AS VARCHAR), '+', ''), ',', '') AS INTEGER) AS Installs_Cleaned
        FROM 'Google_playstore_apps.csv'
        WHERE CAST(REPLACE(REPLACE(CAST(Installs AS VARCHAR), '+', ''), ',', '') AS INTEGER) <= (
        SELECT PERCENTILE_CONT(0.10) WITHIN GROUP (ORDER BY CAST(REPLACE(REPLACE(CAST(Installs AS VARCHAR), '+', ''), ',', '') AS INTEGER))
        FROM 'Google_playstore_apps.csv'
        )
        )
        SELECT Category, Rating, Reviews, Size, Installs, Current_Ver
        FROM LeastInstalledApps
        WHERE Rating < 3.5
        ORDER BY App DESC
	
Output:

| Top Installed App               | Least Installed App             |
|:--------------------------------:|:--------------------------------:|
| ![](topinstall.png)             | ![](leastinstall.png)             |

From the ouput, it is safe to say that the ratings, reviews, size,  and current version directly impact app installation. The topinstalled app has better ratings, more reviews, better size and current version which impacted the number of downlaods from users.

### Business Insights and conclusion
From the analysis, here are few business lesson App owners and intending app owners should note
- Focus development efforts on the most popular categories to maximize users reach and potential downloads.
- Allocate resources to develop appds in categories with the hihest number of apps , as this indicated high user interest and demand.
- Optimize app development for the most frequently used versions to ensure smooth performance and user experience
- Ensure apps are compatible with the most common versions in order to avoid losing users due to compatibility issues
- Develop apps in categories that have high average raings , as these are categories that are likely to have more satisfied users and better app performance
- Lastly, app owners that their apps have good rating should keep on improving on their standards.
