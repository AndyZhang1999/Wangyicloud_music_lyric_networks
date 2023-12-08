# Chinese Song Clustering using Wangyi Cloud

This crawler efficiently retrieves song lyrics and displays high-frequency lyrics in a word cloud. The specific parameters can be set in the settings.

# What Are the Hot Songs on NetEase Cloud Music? A Python Exercise

> Keywords: Data Visualization, Data Analysis, Python Crawler

## Part I

As a novice in Python, I've embarked on a small project to apply my burgeoning skills. The best way to learn a language is through practice, and completing a project offers a sense of achievement.

My objective was to discover the themes of popular songs on NetEase Cloud Music. To access the hot songs, I first thought of crawling NetEase playlists. Fortunately, NetEase Cloud Music features a playlist (Cloud Music Hot Song List) specifically for current popular songs.

To determine the themes, I turned to the lyrics. Each song title in the playlist page links to its page, where I could programmatically access each song's lyrics. Since lyrics are not directly crawlable from the song page, I had to find the NetEase Cloud API. Each song has a unique ID that specifies its lyric link. The final output is a table of 200 songs including their IDs, titles, links, and lyrics, saved as a CSV file locally.

The project utilized these libraries: BeautifulSoup, lxml, requests, wordcloud, pkuseg, matplotlib, re, pandas. BeautifulSoup, lxml, and re were for web parsing or character matching, requests for retrieving web pages, pandas for data handling, wordcloud for creating word clouds, pkuseg for segmentation, and matplotlib for image loading.

Starting from this concept, I first wrote a crawler script. After completing the crawler, I developed a main program to run the crawler and download playlist information. The saved CSV looked like this:

![CSV Example](image-20200223150250918.png)

Next, I wrote a data visualization program and linked it with the crawler in the main program. After extensive debugging, the program successfully ran.

The results were word clouds filtered through a stop word list.

![Word Cloud](image-20200224212514172.png)

The analysis revealed a prevalence of emotive onomatopoeia and interjections. Here are some examples:

> doo, oh, ya, da, yeah

Curiously, the word "da" appeared frequently. Upon investigation, I found it in a particular song:

![Song Example](image-20200224002730017.png)

...

This might suggest that the popularity of these songs is due to their use of primal vocalizations that, although basic, are highly engaging and mood-setting.

After filtering out these interjections:

![Filtered Word Cloud](image-20200224212840916.png)

The words "baby" and "world" were the most frequent, followed by "time," "love," "maybe," "future," "leave."

As for interpretations, I leave it to the audience for diverse perspectives.

Top 20 words:

| Top 1-10 | Count | Top 11-20 | Count |
| :------ | ----: | :------- | ----: |
| baby    |   97 | love     |   50 |
| world   |   68 | time     |   48 |
| ...     |  ... | ...      |  ... |

## Part II

Not content with just this project, I sought to acquire more song information. By searching NetEase Cloud playlists, I could access a list of playlists. For example, searching "rock" yields thousands of playlists, ranked by popularity. Using the NetEase Cloud API to search playlist lists, I obtained a table of playlists and then used the aforementioned method to crawl data. Eventually, I could gather lyrics for thousands of songs, being mindful of duplicates.

All parameters were set in settings.py, facilitating tests across various categories. I was now able to discern the themes in "rock," "folk," "rap," and more.

Here are the results for different genres:

### Rock

I crawled the top 50 "rock" playlists (by popularity), totaling 6215 songs. Of these, 478 had no lyrics, leaving 5737 valid entries.

![Rock Word Cloud](image-20200224220812489.png)

After filtering out interjections:

![Filtered Rock Word Cloud](image-20200224221356112.png)

It seems "time" and "love" are prevalent in rock, along with "baby," "come back," "life," "feel," "heart," "night"—a narrative in an English context. In Chinese lyrics, the themes aren't as apparent; further analysis is needed.

Top 20 words in rock:

| Top 1-10 | Count | Top 11-20 | Count |
| :----- | ----: | :------ | ----: |
| love   | 4926 | home    | 1221 |
| ...    |  ... | ...     |  ... |

### Folk

For "folk," I analyzed the top 50 playlists, with 6506 songs. Excluding 564 songs without lyrics, 5942 entries were valid.

![Folk Word Cloud](image-20200225004811710.png)

After removing interjections:

![Filtered Folk Word Cloud](image-20200225005756964.png)

"Folk" songs frequently mentioned "world" and "love," followed by "girl," "life," "leave."

Top 20 words in folk:

| Top 1-10 | Count | Top 11-20 | Count |
| :----- | ----: | :------ | ----: |
| world  | 1760 | like    |  999 |
| ...    |  ... | ...     |  ... |

### Rap

For "rap," I crawled the top 50 playlists, amounting to 11438 songs. Of these, 7374 had no lyrics, leaving 4064 valid entries.

![Rap Word Cloud](image-20200225003346657.png)

After removing interjections:

![Filtered Rap Word Cloud](image-20200225002013991.png)

"Love" was predominant, followed by "baby," "back." Regardless of the language or genre, everyone seems concerned about "time."

![Filtered Non-English Rap Word Cloud](image-20200225024053290.png)

Top 20 words in rap:

| Top 1-10 | Count | Top 11-20 | Count |
| :----- | ----: | :------ | ----: |
| نى     | 2136 | 喜欢    | 1112 |
| ...    |  ... | ...     |  ... |

## Part III

The effectiveness of segmentation and word clouds depends on text cleaning and the choice of segmentation tools. Text cleaning should remove excess symbols and spaces, especially before generating word clouds. Segmentation results directly impact word cloud outcomes. Initially, I used jieba for segmentation, but the results were unsatisfactory. Switching to Peking University's pkuseg improved both the quality and performance of segmentation.

Some lessons learned during coding:

**About Crawling:** In the `get_lyric()` function, consider when lyrics are missing. If skipped, this leads to mismatched items in the CSV and missing tail data. Setting it to a null value leads to {float}NaN, a floating-point null value. Lyrics should consistently be str type; interspersing other types causes issues later.

**About Python:** Avoid `.remove()` for filtering common words during segmentation; it severely impacts performance, especially with large texts. Instead, use a new list and `.append()`.

**About Regular Expressions:** Avoid excessive 'or' operations in regex replacements. Unnecessary 'or' operations can be replaced with other methods, like handling invalid words/characters via a stop word list.

**About Stop Word Lists:** Ensure English words in the list are either all uppercase or lowercase, and match segmentation accordingly with `.upper()` or `.lower()`.

**About Tool Usage:** While using the wordcloud library, read the documentation thoroughly. The `.generate()` method doesn't always respect word frequency. For frequency-based word clouds, use `.generate_from_frequencies()`.

The code is available on Github for those interested.
