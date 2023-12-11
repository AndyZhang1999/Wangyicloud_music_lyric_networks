# Chinese Song Clustering using Wangyi Cloud

This crawler efficiently retrieves song lyrics and displays high-frequency lyrics in a word cloud. The specific parameters can be set in the settings.

# What Are the Hot Songs on Wangyi Cloud Music? A Python Exercise

> Keywords: Social Networks, Data Analysis, Python Crawler

## Part I

My objective was to discover the themes of popular songs on NetEase Cloud Music. To access the hot songs, I first thought of crawling NetEase playlists. Fortunately, NetEase Cloud Music features a playlist (Cloud Music Hot Song List) specifically for current popular songs.

To determine the themes, I turned to the lyrics. Each song title in the playlist page links to its page, where I could programmatically access each song's lyrics. Since lyrics are not directly crawlable from the song page, I had to find the NetEase Cloud API. Each song has a unique ID that specifies its lyric link. The final output is a table of 200 songs including their IDs, titles, links, and lyrics, saved as a CSV file locally.

The project utilized these libraries: BeautifulSoup, lxml, requests, wordcloud, pkuseg, matplotlib, re, pandas. BeautifulSoup, lxml, and re were for web parsing or character matching, requests for retrieving web pages, pandas for data handling, wordcloud for creating word clouds, pkuseg for segmentation, and matplotlib for image loading.

Starting from this concept, I first wrote a crawler script. After completing the crawler, I developed a main program to run the crawler and download playlist information. The saved CSV looked like this:

![image](https://github.com/AndyZhang1999/Wangyicloud_music_lyric_networks/assets/90740478/d8be3d25-7065-4b95-8913-de12e43f9427)


Next, I wrote a data visualization program and linked it with the crawler in the main program. After extensive debugging, the program successfully ran.

The results were word clouds filtered through a stop word list.

![image](https://github.com/AndyZhang1999/Wangyicloud_music_lyric_networks/assets/90740478/da52ddc8-84ce-4cac-af5f-f825221e89cf)


The analysis revealed a prevalence of emotive onomatopoeia and interjections. Here are some examples:

> doo, oh, ya, da, yeah

Curiously, the word "da" appeared frequently. Upon investigation, I found it in a particular song:

![image](https://github.com/AndyZhang1999/Wangyicloud_music_lyric_networks/assets/90740478/9752096c-2ee5-4df1-9539-bf1af8821d03)


...

This might suggest that the popularity of these songs is due to their use of primal vocalizations that, although basic, are highly engaging and mood-setting.

After filtering out these interjections:

![image](https://github.com/AndyZhang1999/Wangyicloud_music_lyric_networks/assets/90740478/0fa1b424-2a57-4904-a168-e586f5ddfb3b)


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

![image](https://github.com/AndyZhang1999/Wangyicloud_music_lyric_networks/assets/90740478/fe137ca6-09fc-4e41-81b8-4612761391e7)


After filtering out interjections:

![image](https://github.com/AndyZhang1999/Wangyicloud_music_lyric_networks/assets/90740478/09661abe-28be-4403-ad1d-4cfa4b91ba2b)


It seems "time" and "love" are prevalent in rock, along with "baby," "come back," "life," "feel," "heart," "night"—a narrative in an English context. In Chinese lyrics, the themes aren't as apparent; further analysis is needed.

Top 20 words in rock:

| Top 1-10 | Count | Top 11-20 | Count |
| :----- | ----: | :------ | ----: |
| love   | 4926 | home    | 1221 |
| ...    |  ... | ...     |  ... |

### Folk

For "folk," I analyzed the top 50 playlists, with 6506 songs. Excluding 564 songs without lyrics, 5942 entries were valid.

![image](https://github.com/AndyZhang1999/Wangyicloud_music_lyric_networks/assets/90740478/3c8232b3-3e5f-4a7c-83c8-e861aa9eb110)


After removing interjections:

![image](https://github.com/AndyZhang1999/Wangyicloud_music_lyric_networks/assets/90740478/3c3e62d3-4e14-4f52-8383-99a276b75e06)


"Folk" songs frequently mentioned "world" and "love," followed by "girl," "life," "leave."

Top 20 words in folk:

| Top 1-10 | Count | Top 11-20 | Count |
| :----- | ----: | :------ | ----: |
| world  | 1760 | like    |  999 |
| ...    |  ... | ...     |  ... |

### Rap

For "rap," I crawled the top 50 playlists, amounting to 11438 songs. Of these, 7374 had no lyrics, leaving 4064 valid entries.

![image](https://github.com/AndyZhang1999/Wangyicloud_music_lyric_networks/assets/90740478/598b389d-1ea4-47ff-ab1c-78b3c442b257)


After removing interjections:

![image](https://github.com/AndyZhang1999/Wangyicloud_music_lyric_networks/assets/90740478/60b64d7e-6471-479f-8b64-ade6619832bd)


"Love" was predominant, followed by "baby," "back." Regardless of the language or genre, everyone seems concerned about "time."

![image](https://github.com/AndyZhang1999/Wangyicloud_music_lyric_networks/assets/90740478/064fc9a8-b5a8-44bf-993e-258f00c95a3d)

Top 20 words in rap:

![image](https://github.com/AndyZhang1999/Wangyicloud_music_lyric_networks/assets/90740478/06611460-e19a-464d-8d40-996f3fb68b6d)

![image](https://github.com/AndyZhang1999/Wangyicloud_music_lyric_networks/assets/90740478/4ac6065a-4cb2-4020-8005-e2e25b901dae)

![image](https://github.com/AndyZhang1999/Wangyicloud_music_lyric_networks/assets/90740478/3e6406ee-8f38-40af-87e3-1d79e43a49a8)

## Part III

The effectiveness of segmentation and word clouds depends on text cleaning and the choice of segmentation tools. Text cleaning should remove excess symbols and spaces, especially before generating word clouds. Segmentation results directly impact word cloud outcomes. Initially, I used jieba for segmentation, but the results were unsatisfactory. Switching to Peking University's pkuseg improved both the quality and performance of segmentation.

Some lessons learned during coding:

**About Crawling:** In the `get_lyric()` function, consider when lyrics are missing. If skipped, this leads to mismatched items in the CSV and missing tail data. Setting it to a null value leads to {float}NaN, a floating-point null value. Lyrics should consistently be str type; interspersing other types causes issues later.

**About Python:** Avoid `.remove()` for filtering common words during segmentation; it severely impacts performance, especially with large texts. Instead, use a new list and `.append()`.

**About Regular Expressions:** Avoid excessive 'or' operations in regex replacements. Unnecessary 'or' operations can be replaced with other methods, like handling invalid words/characters via a stop word list.

**About Stop Word Lists:** Ensure English words in the list are either all uppercase or lowercase, and match segmentation accordingly with `.upper()` or `.lower()`.

**About Tool Usage:** While using the wordcloud library, read the documentation thoroughly. The `.generate()` method doesn't always respect word frequency. For frequency-based word clouds, use `.generate_from_frequencies()`.

The code is available on Github for those interested.
