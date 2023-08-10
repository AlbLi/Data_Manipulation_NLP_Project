# Biomedical papers content manipulation and NLP model generation
This project obtains data from biomedical research papers. Our team extracted features from those articles' metadata as a start to see if those features correlate with the falsified content manipulation. Besides, the project involves with GPT-2 model to generate fake papers after training the model.

## Part One
Abstract
The dataset “Bik et al 2017 paper” we used as the original data in the project includes 200 biomedical research papers. Our projects can be summarized as four significant tasks: collecting extra data, extracting & incorporating features, applying Tika similarity, and interpreting information.

Collecting Data and Extracting Features:
In addition to the original research dataset, our team collected data such as the author’s universities/institutions, research paper stats, institutions’ locations, and UNDP’s (United Nations Development Programme) HDI (Human Development Index ) index. 

We utilized the Selenium package to extract data on universities/institutions from the ResearchGate website. This involved parsing the HTML using Xpath to locate the desired information. However, in some instances, the initial Xpath does not contain the required data, necessitating the addition of further Xpaths to retrieve this feature. The extracted data is saved in the text/csv MIME format. Previously, we attempted to use BeautifulSoup for extraction but encountered difficulties resulting in a switch to Selenium for web scraping. Although the code is less complex than Beautiful Soup, it takes approximately two hours to complete the web scraping process.

After getting the feature of “university/institution,” we used this feature as the key to web crawl Wikipedia by accessing the Beautiful Soup library  to find each location (countries/regions) corresponding to “universities/institutions.” We exported it as “2_5.csv”, an intermediate dataset waiting for other features to be added. It should be noted that this “2_5.csv” is not included in our final three datasets used to calculate similarities.

However, since there is still a little missing information obtained by purely web crawling, we did manual data cleaning for the column “Location_list” and created dataset “2_5_cleaned.csv” with an updated column “Location” to ensure a good quality dataset for generating additional predictors for our third dataset.

 ![image](https://github.com/KaiyiSss/Big_data_project/assets/60586553/a6cc4443-caf3-4a5f-bcfc-7f99883d3c62)
Figure 1. 2_5_cleaned.csv.


The MIME type of data in the feature “Location” is “text/javascript.” Besides, we collected the research paper stats by scrawling the “ResearchGate” website using the Selenium library. This includes bypassing some standard web scraping detectors activated by the high website access frequency.  After obtaining the data, we generated our dataset: “research_interest_citation.csv.” The MIME type of it is application/x-javascript. This dataset generated features “research interest” and “citation.” 

Then, we download the UNDP’s HDI dataset from its official website, named as ‘HDR21-22_Composite_indices_complete_time_series.csv’.

This dataset calculates the annual HDI from 1990-2021 for 195 countries and regions. The HDI is a geometric mean of normalized indices for each of the following three dimensions: a long and healthy life, being knowledgeable and having a decent standard of living. 

  ![image](https://github.com/KaiyiSss/Big_data_project/assets/60586553/fac3be33-4d65-4303-844b-b77b4dda6cea)
Figure 2. Human Development Index and its components.

 ![image](https://github.com/KaiyiSss/Big_data_project/assets/60586553/5443645b-3505-486c-a5a1-f7058a366002)
Figure 3. ‘HDR21-22_Composite_indices_complete_time_series.csv’ sample.

As the UNDP visualizes it as an interactive chart whose top level MIME type would be multipart since it encapsulates ‘image/gif’ and ‘text/html’.

 ![image](https://github.com/KaiyiSss/Big_data_project/assets/60586553/3860a64f-07df-4a86-a9ed-e15bcf6dc9df)
Figure 4. UNDP website screenshot.

This dataset could be downloaded from this official website as ‘HDR21-22_Composite_indices_complete_time_series.csv’

For this dataset, we want to identify the HDI index for each row. That is, we locate the HDI index in the above csv file, according to the “Location” column and “Year” column of the intermediate ‘2_5_cleaned.csv’ file.

So firstly, we first reformat the country and region name in the  ‘2_5_cleaned.csv’ to make sure they conform to the UNDP format, so they could be located in the UNDP - HDI dataset. For instance, we reformatted ‘Scotland, UK’ as ‘United Kingdom’, and similarly ‘Hong Kong’ to ‘Hong Kong, China (SAR)’ and ‘South Korea’ to ‘Korea (Republic of)’.

Then, we locate the HDI index for each row as is discussed above, and export our final dataset “third_dataset.csv”.
 
Information Interpretation and Tika Similarity:
To process Tika Similarity, we prepare three datasets: “Bik dataset.csv,” “second.csv,” and “third_dataset.csv.”  

Compared to the original dataset, the “second.csv” adds the features “university/institution”, “Research Interest Score” and “Citations (the number of citations). Based on the “second.csv,”  the “third_dataset.csv” updates extra features “Location” and “index.” 

 ![image](https://github.com/KaiyiSss/Big_data_project/assets/60586553/9caf737c-17ee-4823-8f7c-9e8ab156cecf)
Figure 5. updated_bik.csv

 ![image](https://github.com/KaiyiSss/Big_data_project/assets/60586553/329800c8-4eb4-49b9-a744-75825ba6533a)
Figure 6. Second.csv

![image](https://github.com/KaiyiSss/Big_data_project/assets/60586553/a4beb59e-36cb-45d0-bf88-3239706fb93d)
Figure 7. Third_dataset.csv

The institution/universities feature can be used to retrieve the location of these institutions, which in turn will be used to correlate with the UNDP HDI index to answer our research questions. 


## Q1: How does the influence of papers/topics engage in media manipulation? Will papers with higher scores tend to manipulate content?

The Research Interest Score is a measured index generated by “unique ResearchGate members, recommendations on ResearchGate, and citations (excl. self-citations)” to evaluate the impact of the research within the scientific community. Because the number of citations is also an important indicator to measure the influence of research papers, we combined both features for their potential to interpret the significance of the papers and the topics related to scientific fields. In this case, compared to the previous dataset, “second.csv” can answer questions: How does the influence of papers/topics engage in paper manipulation? Will papers with higher scores tend to manipulate content?

By correlation analysis, we set up “Research Interest Score” as the predictor to correlate multiple “manipulation” columns. The computed correlation coefficient corresponding to ‘0’ ‘1’ ‘2’ ‘3’ equals to ‘-0.0307’ ‘-0.113785’ ‘0.026687’ ‘0.087873.’ By this result, the research interest score has the strongest correlation to the ‘3’ manipulation category. The statistics information about the category ‘3’is: mean= 51.48 and standard deviation= 126.26.	
                              	          Screeshot_correlation coefficient                                                                               Statistics Information	
RQ2: Is there any correlation between the citation number and the HDI index?
When looking into this final dataset, we propose a second question: is there any correlation between the citation number and the HDI index? 

To be more specific, the number of citations of a paper can reflect the quality and validity of the research and hence indicates the academic reputation. So, do countries with higher HDI index possess more papers with higher citation numbers? Or, to take one step further, do researches from countries with higher HDI index enjoy better academic reputation?

After running a correlation test between citation numbers under the “Citations” column and the index under the “index” column, we get the correlation coefficient of 0.0064186141875820675, which is too small to support our hypothesis.


## Apach Tika Visulization:
Edit distance:
After calculating the edit distance among the three datasets, we find that the second and third datasets are most similar, whose similarity score is 0.814815. The similarity score between first and third datasets is 0.796296, that between the first and second datasets is 0.777778.

![image](https://github.com/KaiyiSss/Big_data_project/assets/60586553/1653d0d5-713b-4260-bd1c-bb749ce164a4)
Figure 8. Edit distance circle packing visualization

Cosine distance:
After calculating the cosine distance among the three datasets, we find that the results are highly alike. The similarity between  the second and third datasets is still the highest, reaching 1.0, indicating that they are entirely the same. The similarity between the first and second datasets, and between the first and third datasets remain the same and soar high as well, reaching 0.999967.

And we get a similar visualization in the combination cluster viz and circle packing viz.

 ![image](https://github.com/KaiyiSss/Big_data_project/assets/60586553/6a1dc00e-a9a6-4bfe-b863-94a7adc18a43)
Figure 9. Cosine distance circle packing visualization	

![image](https://github.com/KaiyiSss/Big_data_project/assets/60586553/34ba9dd0-d5b4-4aa0-ac1d-ad4a17798ee1)
Figure 10. visualization

Jaccard distance:
After calculating the jaccard distance among the three datasets, we find that the results are identical. The results are 0.538462.
 ![image](https://github.com/KaiyiSss/Big_data_project/assets/60586553/a1a077b9-aa2b-4aed-a384-320580466b19)
Figure 11. Jaccard distance cluster visualization

Similarity Measurement Comparison:
We believe that the cosine similarity is the most accurate one. The cosine similarity measures the 214 rows of our final document, so it is reasonable that the similarity scores are all very high. And since the modification on the second dataset is smaller than that on the first one, we can understand that the similarity between the second and third datasets is bigger than the other two scores.

The edit distance calculation operates on sequences of characters and measures the minimum number of operations required to transform one string into another. So, it does not make that much sense that the similarity between the first and the third datasets is greater than that of the first and second datasets. So are the results of the jaccard similarity, which remain all the same.

Thoughts on Apache Tika
	Apache Tika is a power tool that supports a wide range of file formats, such as csv and json. However, in this project, when calculating the similarity between files, Sklearn is faster and easier to implement. For instance, when using Apache Tika, we have to jump back and forth between files to find the specific .py file for certain similarity measures.

Summary
In this report,we incorporated multiple datasets to enrich our analysis, including university information, citation counts, and research interests index from ResearchGate, country data via Wikipedia, and human development indices from UNDP dataset. Additionally, we conducted a comprehensive analysis of academic papers using various distance methods, including Jaccard Similarity, Edit Distance, and Cosine Similarity, to assess their metadata similarities using Tika-similarity package without delving into their content. We successfully visualized the clustering results using hierarchical circle packing, providing insights into the relationships among the papers. The correlation coefficient between the media manipulation and research interest shed light on unexplored aspects of media forensics data and enabled us to explore how the influence of the research paper interact with media manipulation.


# Part 2
## 1. What did the GPT-2 generated texts look like?
The types of papers GPT-2 generated can be summarized as follows. The significant steps our team processed the tasks included collecting data, training models, and generating papers. In the step of collecting data, our team web scrap 214 original papers along with the additional 250+ papers from the reference lists from the USC library by running the file ‘usc_library_papers.ipynb’ and ‘get_references_papers_with_title.ipynb’. Finally, we parse 214 original papers with Apache Tika due to the limited memory. 

We fine-tuned three models in total, one based on the raw Tika extraction “result_USC.csv” file, one based on the cleaned “trial4_5.csv”, and the other based on the cleaned and latex structured “final_train.csv” file. The output from the first model contains much more random texts/numbers/symbols than the other two. As for the latter two, even though the final model was trained on a much-structured training dataset, its outputs are not structured, as it didn’t learn and differentiate the title from the content as intended. Thus, the outputs of the second and third aren’t that different, except that a few outputs of the third model contain solely latex symbols, which were not included in the second model’s training dataset.
 
Meanwhile, generating all 500 papers simultaneously is impossible due to limited memory. Instead, our team developed multiple batches not at the same time to save memory and merged those batches in the end. The results after generating by GPT-2 show various types of content. First, a few articles look like actual research papers. These papers have introductions, content, and conclusions. The main structure doesn’t make obvious mistakes, while most sentences don’t make sense in readability. Second, papers generated by GPT-2 show a high frequency of repeated straightforward content due to each generation. The repeated content usually only focuses on a specific paper section, like references and author information being duplicated many copies until reaching the regular length of a research paper. Third, even if not-generated articles include a few complete sentences, these sentences cannot deliver consistent ideas longer than one paragraph. In general, GPT-2 cannot infer, summarize, or reason the long content it learns.

## 2. Were they believable?
GPT-2 generated articles have a low believability due to their lack of reasoning text. Based on fake papers we acquired from GPT-2, these articles cannot maintain the consistency of ideas. Thus, even if most texts are in the same paper, GPT-2 cannot make sentences interact well with each other or generate reliable content. 

Also, whether we train the dataset with latex format or not, the output looks pretty random and unstructured, supporting our argument that they are unbelievable.


## 3. Would your associated ancillary features from assignment one have been able to discern what was false or not?
No. As indicated by the  results of the correlation test of our Homework 1, there is no correlation between the media manipulation and the features we collected. Also, those ancillary features were not utilized either for fake data generation or the get-2-simple model fine-tuning process. Plus, we generate the new fake metadata according to the statistical distribution of the third_trial.csv dataset. Thus, it is reasonable to assume that that ancillary feature from assignment one will not be helpful to discern what was false or not.

Yet, theoretically, we can detect duplications through metadata like the location we collected in HW1 and combine it with the time metadata, so we can see if the scholars of some countries are more likely to falsify their papers during a specific time period, say during the graduation season. So, next time we have a new row of data regarding a research paper, we can use its metadata to discern whether it is fake or not.



## 4. How much do you think media falsification is solvable using ancillary metadata features or using actual content-based techniques? Is one better than the other?
Both techniques can help solve media falsification issues with their comparative advantages and disadvantages. We can expand the interested features for ancillary metadata features to make it more comprehensive. For example, we could include more detailed information regarding the lab itself or the authors' previous publications to determine the paper's quality. More metadata provided would give us a more accurate prediction of the paper. One clear advantage of this method is the time it could save compared with the actual content-based techniques, and it doesn’t matter what language was used as well. Also, it is easier on the memory as well. However, some disadvantages could include the difficulty of collecting those metadata, the low accuracy, or strong bias when just a few metadata are collected. 

As for actual content based techniques, it is clear that this would give us the most accurate result, provided that we have a vast dataset to compare. However, it is firstly very costly to train an accurate model to detect complex duplication types, meaning that the model cannot only compare the paper at hand with the dataset but also have a semantic understanding of the paper after removing possible additional information designed to misguide the model. Plus, it would require a solid infrastructure to save the data, ensuring fast and stable data transformation, and can accurately understand multiple languages in an academic setting. 

	Thus, we believe it is challenging to pinpoint which model is better than the other without specifying essential details such as the designed purpose, the resources available, and the required accuracy. But in an ideal situation for both techniques, the content-based techniques should have higher accuracy than ancillary metadata features when dealing with media falsification.



5. What other types of datasets could have been used to generate the falsified
papers? Pick at least two datasets from distinct MIME types.
We could use other datasets of videos or audio to generate the falsified papers. We could use the author's name or affiliation title as the keyword to search for videos on Youtube or other video-dominant platforms. We can use BeautifulSoup or Youtube api to download relevant clips, use packages like ffmpeg-python to transform videos into audio, then use Speech_recognition to transcribe it into text further. Or, we can scrape the caption directly through the three-dot menu below the video →. Click open transcript → scrape the transcript. Or else, we can use youtube-transcript-API for scraping as well.
Similarly, we can use the author name or affiliation title as the keyword to search for audio on Podcast or other audio-dominant platforms. We can use the get podcast package to scrape audio from Podcast and then use Speech_recognition to transcribe it into text for further analyses. 



## 6. What other sorts of “backstopping” would be required to generate a believable
the paper trail for scientific literature?
	We can segment the input files according to abstracts, introductions, results, references, etc., before we feed them into gpt2. In this case, the model would be more fine-tuned and gives us each part with the correct format. Otherwise, it may generate a paper of references only or one with keywords only.

	Moreover, we can feed different sections to gpt2 in an accumulative way. Take Homework 2 as an example. We can first train the model with the 200 original  titles only and generate 500 titles. Then we put all the 700 titles and the 200 introductions into the training dataset and asked gpt2 to generate 500 papers with tiles and introductions. This way, we can have more semantic coherence between different sections of the paper while keeping it structured.

Besides, semi-supervised learning can potentially improve the performance of GPT-2 paper generation. After generating papers of each batch, the result provides ideas about how GPT-2 processes information and where the deficiency is. We can remove or replace the sections that probably cause that messy information to confuse the model. As the current task concerns biomedical topics research papers, the first step to tackling semi-supervised learning is finding the commonalities among these papers to find whether some terms, structures, and experiments often appear in the same literature. In this case, these commonalities are labels to show the classic and standardized formats of research papers in the specific field. These formats also can be customized by the actual demand to correct information confusing GPT-2 model.
