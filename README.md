## Name: PRAVEEN K
## Reg.no: 212223040152
## Netflix Shows & Movies

## Aim

To analyze Netflix dataset and compare movies vs TV shows, top producing countries, and release year trends.

## Procedure / Algorithm

  1)Load dataset (netflix_titles.csv).
  
  2)Count movies vs TV shows.
  
  3)Group by country → top contributors.
  
  4)Create pivot table (release year vs type).
  
  5)Visualize with bar & line charts.
  

## Program

## How many titles (Movies vs. TV Shows) are there?

```
import numpy as np
import pandas as pd

url="https://raw.githubusercontent.com/allenkong221/netflix-titles-dataset/main/netflix_titles.csv"
df=pd.read_csv(url)
df.head()

titles_count = df.groupby('type')['type'].count()
print(titles_count)
```

## Output:
<img width="1054" height="506" alt="image" src="https://github.com/user-attachments/assets/0d480fd8-e2ce-4e10-a3cd-d721f8f21903" />

<img width="537" height="171" alt="image" src="https://github.com/user-attachments/assets/66bb4e5a-6e11-46d5-a2b9-5d5512517f17" />


## What’s the distribution of content types across countries?
## Which country do you expect has the most Netflix content?

```
country_type_distribution = df.pivot_table(
    index='country',
    columns='type',
    values='title',
    aggfunc='count',
    fill_value=0
)

country_type_distribution.head()

country_type_distribution['Total'] = (
    country_type_distribution['Movie'] + country_type_distribution['TV Show']
)

country_type_distribution.sort_values('Total', ascending=False).head()


```

## Ouptut
<img width="693" height="487" alt="image" src="https://github.com/user-attachments/assets/06404a8a-4531-4379-b569-1aa9382a2bd6" />
<img width="708" height="404" alt="image" src="https://github.com/user-attachments/assets/8e909082-320d-4339-a25a-79a8c371e317" />


## What are the top 5 directors by number of titles?

```

top_directors = df['director'].value_counts().head(5)
print(top_directors)

```

## Output:
<img width="592" height="233" alt="image" src="https://github.com/user-attachments/assets/a4f38cf2-13ef-4f44-8628-2310b3f109bd" />


## Monthly trends in additions (by type)?

```
df['date_added'] = pd.to_datetime(df['date_added'], errors='coerce')

df['year_added'] = df['date_added'].dt.year
df['month_added'] = df['date_added'].dt.month

monthly_trends = (
    df.groupby(['year_added', 'month_added', 'type'])
      .size()
      .unstack(fill_value=0)
)

monthly_trends.head()

```

## Output:
<img width="663" height="446" alt="image" src="https://github.com/user-attachments/assets/2729c833-e06f-4529-bfb2-c3648f20cc05" />


## Merge expanded genre/cast tables back to main DF for combined insights.

```
genre_df = df[['show_id', 'listed_in']].dropna()

genre_df = genre_df.assign(
    genre=genre_df['listed_in'].str.split(', ')
).explode('genre')

cast_df = df[['show_id', 'cast']].dropna()

cast_df = cast_df.assign(
    actor=cast_df['cast'].str.split(', ')
).explode('actor')

merged_df = (
    df[['show_id', 'type', 'title']]
    .merge(genre_df[['show_id', 'genre']], on='show_id')
    .merge(cast_df[['show_id', 'actor']], on='show_id')
)

merged_df.head()


```


## Output:

<img width="920" height="405" alt="image" src="https://github.com/user-attachments/assets/607397d8-d444-47d7-9b17-cb91b8726823" />



## Result

Thus the program was executed and verified successfully.
Helps Netflix in content planning & investments.
