---
layout: post
title:  "Lights, Camera, Data! A Data-Driven Exploration of the IMDb Top 250 Movies"
date: 2024-10-31
description: This blog post explores IMDb Top 25 Movies to uncover what makes a film truly iconic. By curating and analyzing data through web scraping and APIs, we delve into genres, awards, ratings, box office, and decades of cinema history to uncover trends and insights about the world's most beloved movies.
image: "/assets/img/imdb250topmovies.webp"
display_image: false
---
# Introduction

Have you ever wondered what defines a great movie? Are highly-rated films characterized by their genre, awards, ratings, or box office? In this blog post, we dive into the IMDb Top 250 Movies to explore their defining features through data curation, web scraping, application programming interface (API), and exploratory data analysis (EDA).

Using a combination of Python, the IMDb website, and the OMDB API, we created an original dataset and performed analyses to uncover trends and insights about these cinematic masterpieces.

# Question of Interest

What are the defining features of highly-rated movies, and do factors such as genre, awards, ratings, and box office correlate with IMDb top 250 rankings?

# Data Collection and Ethics

## Data Sources

1. [IMDb Top 250 Movies Website](https://www.imdb.com/chart/top/): Movie titles were scraped using BeautifulSoup and Selenium.
2. [OMDB API](https://www.omdbapi.com/): Detailed movie information, such as genres, awards, ratings, and box office, was retrieved via the OMDB API.

## Ethical Considerations

- IMDb's [robots.txt](https://www.imdb.com/robots.txt) allows scraping of publicly available pages like the Top 250 Movies list. The scraping was conducted in accordance with these guidelines, avoiding restricted sections of the website.
- By adhering to the OMDB API's [Terms of Use](https://www.omdbapi.com/legal.htm), the project ensured that the data was obtained legally and within the scope of allowed requests. This included using an API key for authentication.
- To prevent overloading the IMDb website or OMDB API server, the project implemented a sleep function between requests. This introduces delays to mimic human browsing behavior and reduces the risk of being flagged as a bot or causing server strain.

# Data Curation Process

To assemble the IMDb Top 250 Movies dataset, I used a combination of web scraping and an API integration.

**1. Web Scraping IMDb Top 250 Movie Titles:**

- **Selenium:** A Chrome WebDriver was used to open the IMDb Top 250 Movies page to automate a browser session and retrieve the page's dynamically loaded HTML content.
- **BeautifulSoup:** Using BeautifulSoup, I parsed the HTML to extract the movie titles. Each title was located inside specific HTML elements with the relevant class names.

```python
# Set up Chrome WebDriver and retrieve HTML content
driver = webdriver.Chrome(service=ChromeService(ChromeDriverManager().install()))

imdb_url = 'https://www.imdb.com/chart/top/'
driver.get(imdb_url)

driver.implicitly_wait(10)

imdb_page_source = driver.page_source
imdb_soup = BeautifulSoup(imdb_page_source, 'html.parser')

driver.quit()

# Parse HTML content and extract movie titles
imdb_container = imdb_soup.find('ul', {'class': 'ipc-metadata-list ipc-metadata-list--dividers-between sc-a1e81754-0 iyTDQy compact-list-view ipc-metadata-list--base'})
imdb_items = imdb_container.find_all('li', {'class': 'ipc-metadata-list-summary-item sc-4929eaf6-0 DLYcv cli-parent'})

movie_titles = []

for imdb_item in imdb_items:
    movie_title = imdb_item.find('h3', {'class': 'ipc-title__text'}).text.strip()
    movie_titles.append(movie_title)

cleaned_movie_titles = [movie_title.split('. ', 1)[1] for movie_title in movie_titles]
```

**2. Fetching Movie Details with the OMDB API:**

After gathering the cleaned list of movie titles, I used the OMDB API to fetch additional movie details such as the release year, genres, awards, ratings including IMDb rating, Rotten Tomatoes Tomatometer, and Metacritic Metascore, and box office.

- **API Key:** Generate an API key by visiting this [link](https://www.omdbapi.com/apikey.aspx). Sotre the API key securely, and include it as a query parater in the API requests.
- **Request Structure:** For each movie title, an HTTP GET request was sent to the OMDB API with the title as a query parameter.
- **Rate Limiting:** To comply with ethical scraping practices and API usage policies, I introduced a `time.sleep(2)` delay between each request to avoid overwhelming the API server.

```python
# Example Usage: http://www.omdbapi.com/?apikey=YOURAPIKEYHERE&t=MOVIETITLEHERE&type=movie&plot=full

with open('omdb_apikey.txt', 'r') as file:
    omdb_apikey = file.read().strip()

omdb_base_url = 'http://www.omdbapi.com/'

movie_details = []

for cleaned_movie_title in cleaned_movie_titles:
    params = {
        'apikey': omdb_apikey,
        't': cleaned_movie_title
    }

    movie_response = requests.get(omdb_base_url, params=params)

    if movie_response.status_code == 200:
        movie_detail = movie_response.json()
        print(f'Successfully fetched movie details for: {cleaned_movie_title}')

        movie_details.append(movie_detail)
    else:
        print(f'OMDB API request failed for: {cleaned_movie_title} - Status Code: {movie_response.status_code}')

    time.sleep(2)
```

# Dataset Summary

- **Number of Observations:** 250 movies
- **Features:**
    - Movie Title
    - Release Year (Numerical)
    - Genre (Categorical)
    - IMDb Rating (Numerical)
    - Rotten Tomatoes Tomatometer (Numerical)
    - Metacritic Metascore (Numerical)
    - Box Office (Numerical)
    - Total Wins (Numerical)
    - Total Nominations (Numerical)
- **Summary Statistics of Key Numerical Variables:**
    - **IMDb Rating:** The average rating is 8.32, with a standard deviation of 0.23. The ratings range from 8.0 to 9.3, indicating that all movies in the dataset have strong audience appeal.
    - **Rotten Tomatoes Tomatometer:** The average critical score is 90.9%, with a minimum of 68% and a maximum of 100%. These high scores demonstrate the critical acclaim of these movies.
    - **Metacritic Metascore:** The average Metascore is 82.3, with a range from 55 to 100. While most movies have strong critical reviews, some notable variability exists.
    - **Box Office:** The average box office gross is approximately $109 million, but the values range widely from $19,181 to $858 million, highlighting significant differences in commercial success.
    - **Total Wins:** The average number of wins is 44, with some movies winning up to 349 awards, reflecting varying degrees of recognition.
    - **Total Nominations:** On average, movies have 57 nominations, with some achieving as many as 359.

# Exploratory Data Analysis (EDA)

**1. Distribution of Movies by Decade**

The distribution of movies across decades, as shown in the bar chart, reveals a significant increase in the number of IMDb Top 250 Movies from the 1980s onwards. This suggests a bias towards more contemporary films, likely due to better data preservation, shifting audience preferences, or wider global reach.

<figure>
    <img src="{{site.url}}/{{site.baseurl}}/assets/img/Distribution of IMDb Top 250 Movies by Decade.png" alt=""> 
    <figcaption>Distribution of IMDb Top 250 Movies by Decade</figcaption>
</figure>

**2. Average Ratings by Decade**

The line chart compars IMDb, Rotten Tomatoes, and Metacritic ratings by decade. IMDb ratings remain relatively stable over time, suggesting a general consistency in viewer evaluation across decades. Rotten Tomatoes Tomatometers and Metacritic Metascores show a decline from earlier decades, possibly reflecting stricter critical standards or differing audience expectations over time.

<figure>
    <img src="{{site.url}}/{{site.baseurl}}/assets/img/Average Ratings by Decade.png" alt=""> 
    <figcaption>Average Ratings by Decade</figcaption>
</figure>

**3. Genre Analysis**

The "Most Common Genres" chart demonstrates that drama dominates the IMDb Top 250 Movies list, followed by adventure, action, and crime. These genres align with widespread audience preferences for emotionally engaging or thrilling narratives. By contrast, niche genres like musical or film noir are less represented, indicating they may appeal to smaller, specific audiences.

<figure>
    <img src="{{site.url}}/{{site.baseurl}}/assets/img/Most Common Genres in IMDb Top 250 Movies.png" alt=""> 
    <figcaption>Most Common Genres in IMDb Top 250 Movies</figcaption>
</figure>

The "Top 10 Genres by Average IMDb Rating" reveals that genres such as western, music, and crime achieve the highest average IMDb ratings. This suggests that while some genres dominate by volume, others might stand out in terms of audience appreciation, highlighting a distinction between popularity and critical acclaim.

<figure>
    <img src="{{site.url}}/{{site.baseurl}}/assets/img/Top 10 Genres by Average IMDb Rating.png" alt=""> 
    <figcaption>Top 10 Genres by Average IMDb Rating</figcaption>
</figure>

**4. Box Office Analysis**

The "Top 10 Genres by Average Box Office" chart shows that adventure, fantasy, and action lead in terms of box office revenue, underscoring their mass-market appeal. However, these genres do not necessarily dominate the IMDb rating averages, suggesting that box office performance is not a reliable indicator of critical success.

<figure>
    <img src="{{site.url}}/{{site.baseurl}}/assets/img/Top 10 Genres by Average Box Office.png" alt=""> 
    <figcaption>Top 10 Genres by Average Box Office</figcaption>
</figure>

The scatterplots for "Box Office vs. IMDb Rating" and other rating comparisons confirm this trend that a higher box office does not guarantee a higher IMDb rating.

<figure>
    <img src="{{site.url}}/{{site.baseurl}}/assets/img/Box Office vs. IMDb Rating.png" alt=""> 
    <figcaption>Box Office vs. IMDb Rating</figcaption>
</figure>

<figure>
    <img src="{{site.url}}/{{site.baseurl}}/assets/img/Box Office vs. Rotten Tomatoes Tomatometer.png" alt=""> 
    <figcaption>Box Office vs. Rotten Tomatoes Tomatometer</figcaption>
</figure>

<figure>
    <img src="{{site.url}}/{{site.baseurl}}/assets/img/Box Office vs. Metacritic Metascore.png" alt=""> 
    <figcaption>Box Office vs. Metacritic Metascore</figcaption>
</figure>

**6. Correlation Analysis**
    
The heatmap of crrelations between numerical variables provide additional context:
- There is a weak positive relationship between total wins and IMDb ratings, as well as total nominations and IMDb ratings. This suggests that while winning or being nominated for awards can slightly enhance a film's reputation among viewers, these features are not the sole determinants of a high IMDb rating. 
- A moderate positive correlation exists between Rotten Tomatoes Tomatometers and Metacritic Metascores, indicating that these movie platforms and databases might share some alignment in evaluating films.
- There is a weak positive correlation between box office and IMDb ratings, which further verifies that box office plays a minor role in determining a movie's IMDb rating.

<figure>
    <img src="{{site.url}}/{{site.baseurl}}/assets/img/Correlation Analysis Between Numerical Variables.png" alt=""> 
    <figcaption>Correlation Analysis Between Numerical Variables</figcaption>
</figure>

# Insights and Conclusions

## Key Findings
This analysis demonstrates that highly-rated movies share specific traits, such as excelling within certain genres (e.g., drama, crime, and western) and maintaining consistent ratings across decades. However, box office and awards are not always directly tied to high ratings, emphasizing the complexity of what makes a film highly-rated. Audience perception could be influenced by a blend of qualitative and quantitative factors Together, these insights address the question of defining features for top-rated movies and reveal nuanced relationships between genres, awards, ratings, and box office.

## GitHub Repository
Find the full code, dataset, and visulization [here](https://github.com/KetherineWang/post-2-data-curation.git).

## Final Thoughts
Through this project, we demonstrated the power of data curation, web scraping, application programming interface, and exploratory data analysis in understanding the characteristics of highly-rated movies. By analyzing trends across genres, awards, ratings, and box office, we gained a deeper appreciation for what makes a cinematic masterpiece.

What trends surprised you the most? Let me know in the comments below!