# Table of Contents

1. [General](#general)
2. [Literature Reviews](#reviews)
3. [Papers - Table Extraction](#papers_te)
4. [Papers - QA on Tables](#papers_qat)
5. [Applications](#applications)


## General <a name="general"></a>

This README contains Webtables papers and resources. Summaries are by [@workindead](https://github.com/workinDead), to the best of our abilities after reading each paper or testing the system (when available). We welcome pull requests with additional resources, papers, or data.

* Brief Introduction

  Tables are a practical and useful tool in many applications seenarios. Tables can be effectively utilized for collecting and organizing information from multiple sources. With the help of additional operations, usch as sorting, filtering and joins, this information can be turned into knowledge and, ultimately, can be used to support decision-making. Thanks to their convenience and utility, a large number of tables are being produced and are made available on the Web. These tables represent a valuable resource and have been a focus of research for over two decades now. 

  Tables on the web, referred to as web tables henceforth, referred to as *web tables* henceforth, differ from traditional tables (that is, tables in relational databases and tables created in spreadsheet programs) in a number of ways. First, web tables are embedded in webpages. There is a lot of contextual information, such as the embedding page's title and link structure, the surrounding text, etc. that can be utilized. Second, web tables are rather heterogeneous regarding their quality, organization, and content. For example, tables on the Web are often used for layout and navigation purposes.

  Among the different table types, *relational tables* (also referred to as *genuine tables*) are of special interest. These describe a set of entities (such as people, organizations, locations, etc.) along with their attributes. Relational tables are considered to be of high-quality, because of the relational knowledge contained in them. However, unlike from tables in relational databases, these relationships are not made explicit in web tables; uncovering them is one of the main research challenges. The uncovered semantics can be leveraged in various applications, including table search, question answering, knowledge base augmentation, and table completion. For each of these tasks we identify seminal work, describe the key ideas behind the proposed approaches, discuss relevant resources, and point out interdependencies among the different tasks.

* Research Outline

  1. Table Extraction
  2. Table Interpretation
  3. Knowledge Base Augmentation
  4. Table Search
  5. Table Augmentation
  6. QA on Tables

© Zhang, S., & Balog, K. (2019, July). Web table extraction, retrieval and augmentation. In Proceedings of the 42nd International ACM SIGIR Conference on Research and Development in Information Retrieval (pp. 1409-1410).


## Literature Reviews <a name="reviews"></a>

* [Web Table Extraction, Retrieval, and Augmentation: A survey](https://arxiv.org/abs/2002.00207) Most up-to-date literature review (Jan. 2020), covering six table-related information access tasks: Table Extraction, Table Interpretation, Table Search, Table Augmentation, Knowledge Base Augmentation, Question Answering
* [Ten Years of WebTables](http://web.eecs.umich.edu/~michjc/papers/p2140-cafarella.pdf) WebTables, the very first system to identify  databases from HTML tables. This paper not only uncovers sub-projects within a decade but also discusses the real impact of this system such as production corpus construction, google tables and etc. 

## Papers -  Table Extraction <a name="papers_te"></a>

Based on three main types of web tables, approaches for table extraction task are organized below:

1. web tables

* Michael J. Cafarella, Alon Halevy, Daisy Zhe Wang, Eugene Wu, and Yang Zhang. 2008. WebTables: Exploring the Power of Tables on the Web. Proc. of VLDB
Endow. 1, 1 (Aug. 2008), 538–549.
* Michael J. Cafarella, Alon Y. Halevy, Yang Zhang, Daisy Zhe Wang, and Eugene Wu 0002. 2008. Uncovering the Relational Web. In Proc. of WebDB ’08.
*  Julian Eberius, Katrin Braunschweig, Markus Hentsch, Maik Thiele, Ahmad Ahmadov, and Wolfgang Lehner. 2015. Building the Dresden Web Table Corpus: A Classication Approach. In 2nd IEEE/ACM International Symposium on Big Data Computing, BDC 2015, Limassol, Cyprus, December 7-10, 2015. 41–50.

2. Wikipedia tables

* Chandra Sekhar Bhagavatula, Thanapon Noraset, and Doug Downey. 2015. TabEL: Entity Linking in Web Tables. In Proc. ISWC ’15. 425–441.

3. spreadsheets

* Zhe Chen and Michael Cafarella. 2013. Automatic Web Spreadsheet Data Extraction. In Proc. of SS@ ’13. 1–8.

## Papers -  QA on Tables <a name="papers_qat"></a>

1. Traditional methods:

   **Semantic Parsing**

- [Compositional semantic parsing on semi-structured tables](https://arxiv.org/abs/1508.00305) Question answering from tables is seen as a semantic parsing task where the question is translated to a logical form that can be executed against a knowledge base.

2. Creative methods: 

   **Neural Network**

- [TAPAS: Weakly Supervised Table Parsing via Pre-training](https://arxiv.org/abs/2004.02349)  TAPAS find answers without generating logical forms, by extending the [BERT](https://ai.googleblog.com/2018/11/open-sourcing-bert-state-of-art-pre.html) architecture to encode the question jointly along with tabular data structure, resulting in a model that can then point directly to the answer.[Github Repo](https://github.com/google-research/tapas)



## Applications  <a name="applications"></a>

应用的基础在于语料库本身的价值，以下表格的应用都基于表格本身包含很多信息

1. Keyword Search Tool for Google

   - Col-subset query - 过滤出有效列

     Query: "city population in U.S." 

     Output: 

     1. Google Search

     [original retrieved wikipedia url](https://en.wikipedia.org/wiki/List_of_United_States_cities_by_population)

     ![Screen Shot 2020-07-06 at 3.46.58 PM.png](http://ww1.sinaimg.cn/large/74c6a0b5gy1gghatw2lydj21ss0r44fb.jpg)

     Google Feature Snippets Applicaiton

   <img src="http://ww1.sinaimg.cn/large/74c6a0b5gy1ggh938nwwmj21bg0vmtdc.jpg" style="zoom:50%;" />

   https://github.com/workinDead/Webtables-Papers/tree/master/QA-on-Tables

   ​		2. Bing Search

   ​		[original retrieved url](https://worldpopulationreview.com/us-cities/) 

   ​		![Screen Shot 2020-07-06 at 3.52.15 PM.png](http://ww1.sinaimg.cn/large/74c6a0b5gy1gghazc3eo3j21n60zwk12.jpg)

   ​	Bing Table Search Applicaiton

   ![Screen Shot 2020-07-06 at 3.50.09 PM.png](http://ww1.sinaimg.cn/large/74c6a0b5gy1gghax4xtz4j214k0xgn1k.jpg)

   - Row-subset query - 过滤出有效行

     Original Retrieved Wikipedia url

     ![original](http://ww1.sinaimg.cn/large/74c6a0b5gy1gghbbu6n1bj20fo176tka.jpg)

     Google Structure Snippets 
   
     ![original](http://ww1.sinaimg.cn/large/74c6a0b5gy1gghbbuvvkdj20oy12etrr.jpg)



2. Keyword Table Search for Microsoft Excel

- 自动补全
- 同义词查找（已删）

对整张表进行提问

![Screen Shot 2020-07-06 at 3.35.20 PM.png](http://ww1.sinaimg.cn/large/74c6a0b5gy1gghapn1fkmj224j0zjki3.jpg)



