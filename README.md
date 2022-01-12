# search-engine-using-solr

We created a search engine using a PHP client and Solr server. The main functions of this project is to show the top 10 resulting pages for any keyword according to different page ranking algorithms, Lucene and PageRank. It also supports autocomplete and spell-correction. A sample, crawled pages dataset is used for the project-confidentiality purposes. These crawled pages were mapped and indexed. Using them, a network graph was created using JSoup and NetworkX libraries that generated an external file for PageRank algorithm. Since Solr already supports Lucene ranking algorithm, we needed to create a different external file for PageRank. The responses from Solr were in JSON, which were then formatted to display on the webpage. For spell-correction implementation, we used Norvig's spelling program; The autocomplete feature is included within Solr.

# ExtractLinks.java

In order to create an outgoing link structure for the crawled and indexed pages, we used JSoup library in Java. With this extracted list, we proceed to next step to complete the ranking process.

# networkX.ipynb

Using the networkX library in python along with the extracted outgoing links, we generate the external file for PageRank in the form of a weighted, directed graph.

# Parser.java

In order to implement the spell-correction program, we need to have a dictionary of words according to our dataset. This text file helps calculate the appropriate edit distance in between keywords.

# SpellCorrector.php

This program is based on the concept of Peter Norvig's spelling correction program. The text file from above is used to generate a serialized dictionary

# index.php

This is the main search engine page, that consists of a text box for keyword input and a radio button to select the type of ranking algorithm to use. When the results are ready, it is displayed in a tabular format.



https://www.youtube.com/watch?v=MQzxBC9Ytqk

