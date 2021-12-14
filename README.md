# search-engine-using-solr

We created a search engine using a PHP client and Solr server. The main functions of this project is to show the top 10 resulting pages for any keyword according to different page ranking algorithms, Lucene and PageRank. It also supports autocomplete and spell-correction. A sample, crawled pages dataset is used for the project-confidentiality purposes. These crawled pages were mapped and indexed. Using them, a network graph was created using JSoup and NetworkX libraries that generated an external file for PageRank algorithm. Since Solr already supports Lucene ranking algorithm, we needed to create a different external file for PageRank. The responses from Solr were in JSON, which were then formatted to display on the webpage. For spell-correction implementation, we used Norvig's spelling program; The autocomplete feature is included within Solr.

ExtractLinks.java
In order to create an outgoing link structure for the crawled and indexed pages, we used JSoup library in Java. With this extracted list, we proceed to next step to complete the ranking process.

networkX.ipynb
Using the networkX library in python along with the extracted outgoing links, we generate the external file for PageRank in the form of a weighted, directed graph.

Parser.java
In order to implement the spell-correction program, we need to have a dictionary of words according to our dataset. This text file helps calculate the appropriate edit distance in between keywords.

SpellCorrector.php
This program is based on the concept of Peter Norvig's spelling correction program. The text file from above is used to generate a serialized dictionary

websrv.php
This is the main search engine page, that consists of a text box for keyword input and a radio button to select the type of ranking algorithm to use. When the results are ready, it is displayed in a tabular format.

Miscellaneous Configuarations
Add the lines to retrieve the data for Title, URL and Description along with the indexed ID in the managed_schema file.

Uncomment the line from managed_schema file.

<copyField source="*" dest="_text_"/>
Add the code in solrconfig.xml to rectify the IO exception error occurred while indexing, if any.

<lib dir="../../extract" regex=".*\.jar" />
<lib dir="${solr.install.dir:../../../..}/contrib/extraction/lib" regex=".*\.jar" />
<lib dir="${solr.install.dir:../../../..}/contrib/dataimporthandler/lib/" regex=".*\.jar" />
<lib dir="${solr.install.dir:../../../..}/dist/" regex="solr-dataimporthandler-.*\.jar" />
<lib dir="${solr.install.dir:../../../..}/contrib/extraction/lib" regex=".*\.jar" />
<lib dir="${solr.install.dir:../../../..}/dist/" regex="solr-cell-\d.*\.jar" />
<lib dir="${solr.install.dir:../../../..}/contrib/clustering/lib/" regex=".*\.jar" />
<lib dir="${solr.install.dir:../../../..}/dist/" regex="solr-clustering-\d.*\.jar" />
<lib dir="${solr.install.dir:../../../..}/contrib/langid/lib/" regex=".*\.jar" />
<lib dir="${solr.install.dir:../../../..}/dist/" regex="solr-langid-\d.*\.jar" />
<lib dir="${solr.install.dir:../../../..}/contrib/velocity/lib" regex=".*\.jar" />
<lib dir="${solr.install.dir:../../../..}/dist/" regex="solr-velocity-\d.*\.jar" />
<requestHandler name="/update" class="solr.UpdateRequestHandler"></requestHandler>
<requestHandler name="/update/extract" startup="lazy" class="solr.extraction.ExtractingRequestHandler" >
  <lst name="defaults">
    <str name="lowernames">true</str>
    <str name="uprefix">ignored_</str>
    <!-- capture link hrefs but ignore div attributes -->
    <str name="captureAttr">true</str>
    <str name="fmap.a">links</str>
    <str name="fmap.div">ignored_</str>
  </lst>
</requestHandler>
Uncomment the line from solrconfig.xml file.

<str name="df">_text_</str>
Use the following parameters for the creation of NetworkX Graph:-

alpha=0.85, personalization=None, max_iter=30, tol=1e-06, nstart=None, weight='weight', dangling=None
Place the external_pageRankFile.txt generated above in data folder of your Solr core.

Add the following two lines in managed_schema file:-

<fieldType name="external" keyField="id" defVal="0" class="solr.ExternalFileField" />
<field name="pageRankFile" type="external" stored="false" indexed="false" />
Add the following two lines in solrconfig.xml file:-

<listener event="newSearcher" class="org.apache.solr.schema.ExternalFileFieldReloader" />
<listener event="firstSearcher" class="org.apache.solr.schema.ExternalFileFieldReloader" />
