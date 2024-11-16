---
layout: post
title:  "Lights, Camera, Data! A Data-Driven Exploration of the IMDb Top 250 Movies"
date: 2024-10-31
description:  
image: "/assets/img/imdb250topmovies.webp"
display_image: false
---
# Introduction
Have you ever wondered what defines a great movie? Are highly-rated films characterized by their genre, runtime, awards, or box office earnings? In this blog post, we dive into IMDb’s Top 250 movies to explore their defining features through data curation, web scraping, and exploratory data analysis (EDA).

Using a combination of Python, the IMDb website, and the OMDB API, we created an original dataset and performed analyses to uncover trends and insights about these cinematic masterpieces.

# Question of Interest

What are the defining features of highly-rated movies, and do factors such as genre, release year, runtime, and awards correlate with IMDb Top 250 rankings?

# Data Collection and Ethics
## Data Sources
1. [IMDb Top 250 Movies Website](https://www.imdb.com/chart/top/): Movie titles were scraped using BeautifulSoup and Selenium.
2. [OMDB API](https://www.omdbapi.com/): Detailed movie information, such as genres, IMDb ratings, box office earnings, and awards, was retrieved via the OMDB API.

## Ethical Considerations
- IMDb's [robots.txt](https://www.imdb.com/robots.txt) allows scraping of publicly available pages like the Top 250 Movies list.
- API requests were limited to avoid server overload, adhering to ethical web scraping practices.

# Data Curation Process
1. **Scraping IMDb Titles**: Selenium and BeautifulSoup were used to retrieve the top 250 movie titles from the IMDb website.
2. **Fetching Movie Details**: Using the OMDB API, details for each movie, such as genres, ratings, and box office earnings, were collected.
3. **Data Cleaning**: Missing or invalid data was handled by removing blank or NaN values and converting numeric fields into consistent formats.
4. **Feature Engineering**: Additional features, such as total wins and nominations, were extracted from the awards data.

# Dataset Summary
- Number of Observations: 250 movies
- Features:
    - Title
    - Year
    - Genre
    - IMDb Rating
    - Rotten Tomatoes Tomatometer
    - Metacritic Metascore
    - Box Office
    - Total Wins
    - Total Nominations

# Exploratory Data Analysis (EDA)
1. **Distribution of Movies by Decade**

    The following bar chart shows the distribution of the IMDb Top 250 movies by decade. The majority of the top-rated movies were released between 1980 and 2020.

<figure>
	<img src="/assets/img/Distribution of IMDb Top 250 Movies by Decade.png" alt=""> 
	<figcaption>Distribution of IMDb Top 250 Movies by Decade</figcaption>
</figure>

2. **Average Ratings by Decade**

    Analyzing the average IMDb, Rotten Tomatoes, and Metacritic ratings by decade revealed interesting trends. Older movies often had higher Rotten Tomatoes scores, while IMDb ratings remained relatively stable.

<figure>
	<img src="/assets/img/Average Rating by Decade.png" alt=""> 
	<figcaption>Average Rating by Decade</figcaption>
</figure>

3. **Most Common Genres**

    Drama emerged as the most frequent genre among the IMDb Top 250 movies, followed by Adventure and Action.

<figure>
	<img src="/assets/img/Most Common Genres in IMDb Top 250 Movies.png" alt=""> 
	<figcaption>Most Common Genres in IMDb Top 250 Movies</figcaption>
</figure>

4. **Top 10 Genres by Average IMDb Rating**

    Western, Music, and Crime genres lead in average IMDb ratings among the top 10 genres.

<figure>
	<img src="/assets/img/Top 10 Genres by Average IMDb Rating.png" alt=""> 
	<figcaption>Top 10 Genres by Average IMDb Rating</figcaption>
</figure>

5. **Top 10 Genres by Average Box Office**

    Action, Adventure, and Fantasy dominate box office revenues among the top 10 genres.

<figure>
	<img src="/assets/img/Top 10 Genres by Average Box Office.png" alt=""> 
	<figcaption>Top 10 Genres by Average Box Office</figcaption>
</figure>

6. **Correlation Analysis**
    
    A heatmap of correlations revealed:
    - Box office earnings positively correlate with total nominations and wins.
    - IMDb ratings have little correlation with box office earnings.

<figure>
	<img src="/assets/img/Correlation Analysis Between Numeric Variables.png" alt=""> 
	<figcaption>Correlation Analysis Between Numeric Variables</figcaption>
</figure>

7. **Average Rating by Decade**
<figure>
	<img src="/assets/img/Average Rating by Decade.png" alt=""> 
	<figcaption>Average Rating by Decade</figcaption>
</figure>

# Insights and Conclusions

## Key Findings
1. **Genres**: Drama is the most common genre, while Western and Music genres achieve the highest IMDb ratings.
2. **Decades**: The 1980s to 2020s dominate the IMDb Top 250 list.
3. **Box Office**: High box office revenue correlates with awards but not necessarily with IMDb ratings.
Ratings: Rotten Tomatoes and Metacritic ratings show higher variability across decades compared to IMDb ratings.

## Next Steps
- Could runtime or language play a role in a movie’s popularity?
- Further analysis could involve sentiment analysis of audience reviews.

# GitHub Repository
Find the full code and dataset [here](https://github.com/KetherineWang/post-2-data-curation.git).

# Final Thoughts
Through this project, we demonstrated the power of data curation, web scraping, and exploratory analysis in understanding the characteristics of highly-rated movies. By analyzing trends across genres, decades, and ratings, we gained a deeper appreciation for what makes a cinematic masterpiece.

What trends surprised you the most? Let us know in the comments below!