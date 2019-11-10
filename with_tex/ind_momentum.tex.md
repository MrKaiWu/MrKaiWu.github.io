## Text-based Industry Reclassification and Industry Momentum

**Project Overview:** This is my graduate thesis at _Renmin University of China_, completed during the spring of 2019. I built a new industry classification for China's listed companies using product & services descriptions disclosed in annual/semi-annual reports. This classification performed well as compared to conventional classifications. Consistent with a behavioral finance perspective, the new industry momentum factor, calculated as the average return of industy peers, was proved to have much stronger predictive power of future stock returns than conventional industry momentum factor.

### 1. Motivation
Conventional industry classifications typically categorize a company into one industry. However, in China a large proportion of listed companies have their business spanned across several industries or sectors. Investors who rely on conventional classifications thus tend to ignore industry-level information relevant to a focal company's secondary business, resulting in delayed incorporation of such information into stock prices — a phenomenon called **limited attention** in behavioral finance literature. Ideally, capturing the information that investors overlook would help better predict future stock returns.

### 2. Methodology: Text-based Industry Classification

Starting from the 2015 annual report (and the 2017 semi-annual report), the China Securities Regulatory Commission(henceforth CSRC) requires listed companies to disclose their business profiles. This section contains a large number of vocabulary related to the company's products and services. If the product & services descriptions of two companies are similar, they probably belong to the same industries. Based on this observation, my study uses text information to redo the industry classification. The specific steps are as follows:

- Webscraping: annual and semi-annual reports are automatically collected from [elangshen](http://www.elangshen.com/)  
- Text cleaning:
  - Extract product & services descriptions using regular expressions; remove tables for they are mostly either irrelavant or boilerplates
  - Tokenize the text and keep nouns
  - Remove high frequency words and rare words, as they introduce noise
  - Remove reports that have few unique words
- Map text into vectors following the Bag-of-Words model: Denote the normalized vector of company $i$ at period $t$ as $v_{i,t}$  
- Cacluate similarity scores between any two comany-period observations
  - Calculate text vector distances using cosine similarity: Denote the cosine similarity between comany $i$ and company $j$ at period $t$ as $CosineSimilarity(i, j)_t = v_{i,j} * v_{j,t}$
  - Calculate similarity scores: $Similarity(i,j)_t = Cosinesimilarity(i, j)_t - Median(\{k \in Companies_t: CosineSimilarity(i, k)_t})$, where $Companies_t$ is the set of companies at period $t$.
- Define **industry peers**:
  - For a focal company $i$, its industry peer at period $t$ is defined as: $IndustryPeer_{i, t} = \{j \in Companies_t: Min(Similarity(i, j)_t, Similarity(j, i)_t) > Threshold_t\}$
  - Use Shenwan Industry Classification (henceforth SWS Ind) to determine the value of $Threshold_t$: set $Threshold_t$ in a way that at period $t$, the number of industry pairs defined by text is approximate to that defined by SWS Ind

SWS Ind is the most popular industry classification among finance practioners in China. This study thus uses SWS Ind as the major benchmark. CSRC has also published industry classification (denoted as CSRC Ind for short), which is the default choice in academic research. But as you will see later, CSRC Ind performs worse than SWS Ind.

When working on this thesis, the annual report for fiscal year 2018 was not completely availble. Consequently, I gathered five periods of reports that had product & services information disclosed. Industry reclassification, namely the process described above on how to define industry peers, was conducted on Apr 30 (Aug 31) when most annual(semi-annual) reports had been made public. To further avoid look-ahead bias, the text for companies that failed to issue reports before the expected date was not updated.

**Table 1** number of companies with at least one industry peer:

| 2015A | 2016A | 2017H | 2017A | 2018H |
|:-----:|------:|-------|-------|-------|
|  2687 |  3035 | 3248  | 3431  | 3501  |

As shown in Table 1, text-based industry covers more than 90% of the entire A-share universe.

**Table 2** descriptive statistics: number of industry peers for a focal company

|      | 2015A | 2016A | 2017H | 2017A | 2018H |
|------|-------|-------|-------|-------|-------|
| mean | 51.95 | 57.62 | 61.61 | 65.91 | 67.03 |
| std  | 36.97 | 43.72 | 43.89 | 52.96 | 52.41 |
| min  | 1     | 1     | 1     | 1     | 1     |
| 25%  | 23    | 23    | 27    | 23    | 26    |
| 50%  | 43    | 46    | 51    | 51    | 53    |
| 75%  | 72    | 81    | 86    | 95    | 94    |
| max  | 215   | 225   | 264   | 284   | 292   |

Table 2 shows the distribution of the number of industry peers a focal company has under the text-based industry classification. For comparison, Table 3 lists the same statistics when using SWS Ind, for the same universe of stocks. Obviously, the order statistics are of comparable scale, which results directly from using SWS Ind as a benchmark.

**Table 3** descriptive statistics: number of industry peers for a focal company defined by SWS Ind

|      | 2015A | 2016A | 2017H | 2017A | 2018H |
|------|-------|-------|-------|-------|-------|
| mean | 54.54 | 60.90 | 62.91 | 67.03 | 67.43 |
| std  | 43.02 | 47.41 | 47.93 | 51.40 | 51.77 |
| min  | 0     | 0     | 0     | 0     | 0     |
| 25%  | 22    | 27    | 29    | 29    | 31    |
| 50%  | 41    | 44    | 47    | 49    | 50    |
| 75%  | 69    | 81    | 87    | 96    | 100   |
| max  | 159   | 177   | 177   | 193   | 194   |


### 3. Qualitative Analysis
#### 3.1 The SWS Inds of Industry Peers 

The table below shows two randomly selected focal companies and the SWS Inds corresponding to their industry peers.

**Table 4** 

| Stock Names and Tickers  | Original SWS Ind         | Frequency of Occurences of SWS Inds among Industry Peers                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Stock Names for Most Similar Industry Peers (Top 10)                                                                                                                                                                           |
|--------------------------|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Phoenix Media 601928.SH  | cultural media           | cultural media [36], computer applications [17], internet media [10], communication equipment [6], marketing communications [4], general retail [3], computer equipment [3], chemicals [2], professional retail [2], commercial property management [2], real estate development [1], optical optoelectronics [1], hotel [1], clothing home textiles [1], textile manufacturing [1], household light industry [1], papermaking [1], packaging and printing [1], audio-visual equipment [1], food processing [1], comprehensive [1]                                                                                                                     | Xinhua Wenxuan, Changjiang Media, Chinese Media, Publishing Media, Central South Media, Chinese Online, Urban Media, China Science Communication, Borui Communication, Southern Media                                    |
| Renfu Medicine 600079.SH | chemical pharmaceuticals | traditional Chinese medicine [44], chemical pharmaceuticals [40], biological products [14], securities [13], banks [9], pharmaceutical business [8], medical services [6], chemicals [5], medical devices [5] , comprehensive [5], animal health [3], special equipment [3], clothing home textile [2], infrastructure [2], food processing [2], chemical fiber [2], real estate development [2], cultural media [1], semiconductor [1], automotive parts [1], park development [1], cement manufacturing [1], water [1], high and low voltage equipment [1], trade [1], motor [1], plastic [1], coal mining [1], metal products [1], internet media [1] | Guilin Sanjin, China Pharmaceutical, Lianhuan Pharmaceutical, Health Yuan, Kangyuan Pharmaceutical, Kunming Pharmaceutical Group, Qianjin Pharmaceutical, Jinhua Shares, Kangzhi Pharmaceutical, Zhejiang Pharmaceutical |

#### 3.2 High Frequency Words in Product & Services Descriptions of Industry Peers

**Table5**

| Stock Name and Ticker          | SWS Ind    | Top 30 High-frequency Words in the Product & Services Descriptions of Industry Peers                                                                                                                                                                                                                                                                                                                                                                                                               |
|--------------------------------|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Northeast Securities 000686.SZ | securities | banks [84], securities [82], financing [81], funds [79], transactions [71], supervision [69], bonds [66], income [65], finance [64], credit [63], Consultation [58], wealth [58], transformation [56], private placement [55], entity [54], compliance [54], RMB [54], feature [53], stock [53], consultant [51] Brokerage [51], futures [51], regulation [50], individual [49], asset management [48], underwriting [48], structure [46], insurance [46], equity [45], investors [45]             |
| Hainan Mining 601969.SH        | mining     | reserves [30], minerals [29], mining [29], mines [28], metals [27], mining [26], mines [26], smelting [24], grades [23], yields [23], Mining area [23], exchange [23], ore [22], mining [22], raw materials [21], exploration [21], copper [20], use [20], quantity [20], geology [20] , mineral processing [20], trade [19], solid [19], non-ferrous metals [18], exploration [18], securities [17], utilization [17], production capacity [17], production capacity [16], chemical industry [16] |






