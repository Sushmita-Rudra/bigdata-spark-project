# bigdata-spark-project

This project will implement the top 10 highest word count in a data text file.

## Data source
The data source file used is at [http://shakespeare.mit.edu/julius_caesar/full.html](http://shakespeare.mit.edu/julius_caesar/full.html).

Then I used the below curl command to get the HTML into a text file.
```
curl "http://shakespeare.mit.edu/julius_caesar/full.html" -o "julius_caesar.txt"
```
## Cleaning up the data text file
To clean up the text file from the html tags and most commonly used english words, I used the below command
```
sed -E 's/\b(the|The|of|Of|and|And|a|A|to|To|in|In|is|Is|you|You|that|That|it|It|he|He|was|Was|for|For|on|On|are|Are|as|As|with|With|his|His|they|They|I|i|said|Said|can|Can|your|Your|when|When|we|We|were|Were|all|All|what|What|not|Not|but|But|word|Word|by|By|had|Had|one|One|or|Or|from|From|have|Have|this|This|be|Be|at|At|there|There|use|Use|an|An|each|Each|which|Which|she|She|do|Do|how|How|their|Their|if|If|will|Will|up|Up|other|Other|about|About|out|Out|many|Many|then|Then|Them|them|these|These|so|So|people|People|could|Could|way|Way|no|No|number|Number|see|See|go|Go|write|Write|more|More|two|Two|look|Look|has|Has|time|Time|into|Into|him|Him|like|Like|make|Make|would|Would|her|Her|some|Some|my|My|than|Than|first|First|water|Water|been|Been|call|Call|who|Who|oil|Oil|its|Its|now|Now|find|Find|long|Long|down|Down|day|Day|did|Did|get|Get|come|Come|made|Made|may|May|part|Part|.|O)\b/''/g'  julius_caesar_cleanedHtml.txt > julius_caesar_cleaned.txt
```

The top 100 most used english words resource reference can be found in the references section of the readme.md file.

## Set up and processing using spark
Spark can be set up in the computer by following step-by-step instrictions provided in [https://github.com/denisecase/setup-spark](https://github.com/denisecase/setup-spark)

After successful set up of spark, run the ```spark-shell``` command to make sure it is working fine.
Then create RDD for the existing text file by
  ```
   val rddCreate = sc.textFile("julius_caesar_cleaned.txt")
  ```

Then I executed the below commands using the higher order functions flatmap, map,reduce etc..
 ```
 val rddSplit = rddCreate.flatMap(eachLine => eachLine.split(" "));
 ```
 
 ```
 val rddMap = rddSplit.map(eachWord =>(eachWord,1));
 ```
 
 ```
 val rddReduce = rddMap.reduceByKey(_+_);
 ```
 Output after reducing:
 
 ```
 Array[(String, Int)] = Array((showshastyspark,"",1), (better.,1), ("veryhappy,"",1), (praise."",1), (Fortunemerry,"",1), ("universalshout,"",1), ("council;,1), ("darefight,,1), (blamethee;,1), (greatdanger,,1), ("baredbosomthunderstone;"",1), (amcompellset"",1), ("runsovereveneyes."",1), (conjure,1), ("crowd;,1), (yetlovewell."",1), ("exaltedthreateningclouds:"",1), (meditates."",1), ("mightygodsdefendthee!,1), ("Herelieseast:,1), ("mightyeaglesfell,,1), ("hardhearts,,1), ("WherePublius?"",1), ("secretRomans,,1), (Hearme,,1), (":"",2), ("Yours,,1), (live,,1), ("morrow,,2), ("shallfuneralspeechblameus,"",1), (death"",1), ("visitplaces;,1), (settingsun,"",1), ("trueriteslawfulceremonies."",1), ('Caesar'?"",1), (spoke,"",1), (sinceputsword--"",1), (dare,,1), ("near,"",1), ("visionfai...
 ```
 
 Then I used the sort method to sort the reduced data in descending order and took the top 10 words,
 ```
 val result = rddReduce.sortBy(_._2, false)
 ```
 
 ```
 result.take(10)
 ```
 
 The result is saved in a text file like below
 ```
 result.saveAsTextFile("resultData")
 ```
 
 ## Data Visualization using chart:
 
 After sorting the data and taking the top 10 mostly used words, I put the result data in excel and created a bar chart which can be seen below
 
 ![Chart](/WordCount.PNG "Chart")
 
 ## References
 1. Curl command [http://www.compciv.org/recipes/cli/downloading-with-curl/](http://www.compciv.org/recipes/cli/downloading-with-curl/)

 1. Sed commands [https://www.cyberciti.biz/faq/howto-delete-word-using-sed-under-unix-linux-bsd-appleosx/](https://www.cyberciti.biz/faq/howto-delete-word-using-sed-under-unix-linux-bsd-appleosx/)
 1. Most 100 english used words [https://www.rypeapp.com/most-common-english-words/](https://www.rypeapp.com/most-common-english-words/)
 1. Spark set up and higher order function [https://github.com/denisecase/setup-spark](https://github.com/denisecase/setup-spark)
 1. Data source [http://shakespeare.mit.edu/julius_caesar/full.html](http://shakespeare.mit.edu/julius_caesar/full.html)
 1. Markdown [https://wordpress.com/support/markdown-quick-reference/](https://wordpress.com/support/markdown-quick-reference/)
 1. flatmap command [https://sparkbyexamples.com/spark/spark-flatmap-usage-with-example/](https://sparkbyexamples.com/spark/spark-flatmap-usage-with-example/)
 
 
