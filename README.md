# Text annotation using NLTK (Natural language toolkit)

This project allows to extreact a text from an iput file and extracts a group of annotations of that text like the lemmatization and stemming etc... which are important process in natural language processing.




## Getting Started

## Prerequisites
* python 3 or above.
* NLTK.
* Numpy.
* Pandas

## What is NLTK?
NLTK is a leading platform for building Python programs to work with human language data. It provides easy-to-use interfaces to over 50 corpora and lexical resources such as WordNet, along with a suite of text processing libraries for classification, tokenization, stemming, tagging, parsing, and semantic reasoning, wrappers for industrial-strength NLP libraries, and an active discussion forum.

## Our task
We will be importing  any text that we want to provide it's annotations, the annotations are:
* Count: the number of occurances of each word.
* Lemma: Lemmatization considers the context and converts the word to its meaningful base form, which is called Lemma.Example: lemmatizing the word ‘Caring‘ would return ‘Care‘.
* Steamma: Stemming is a process that stems or removes last few characters from a word, often leading to incorrect meanings and spelling.	Lemmatization considers the context and converts the word to its meaningful base form, which is called Lemma.
Example:  stemming the word ‘Caring‘ would return ‘Car‘.
* Pos Tag: POS Tagging in NLTK is a process to mark up the words in text format for a particular part of a speech based on its definition and context. Some NLTK POS tagging examples are: CC, CD, EX, JJ, MD, NNP, PDT, PRP$, TO, etc.
* Empty word: a word or morpheme that has no lexical meaning and that functions as a grammatical link or marker like the word "the".
* Named entity: Named entities are persons, locations, organizations, time expressions, etc.


## Code
##### The text
As spcified above, will be extracting annotations from any text, in this example I extracted the text from a .html file and then using regular expressions I was able to extract the text that is in the "<p>" tag and ignore the rest of the file content since I'l only intrested in the text.
```
file_name = 'text.html'

with open(file_name, 'r') as file:
    input_text = file.read()
    paragraphs=re.findall('<p>(\w.+)<\/p>',input_text)
file.close()
```
You can change the regular expression as you want if you are working with another input file, for more informations about regular expressions check : https://regex101.com/.


The next step is to parse our text to sperate it into words and put them in a list so we can get the annotations for each word, for that we use NLTK's pre-defined commands, this process is called Tokenization.
```
all_words=[]

for text in paragraphs:
  arr_of_lines = sent_tokenize(text) #tokenize the text into phrases.
  for line in arr_of_lines:
    word_list = word_tokenize(line) # tokenize the phrases into words.
    for word in word_list:
      all_words.append(word)
```
The output should look like this :
['East', 'Providence', 'should', 'organize', 'its', 'civil', ...]

##### Getting the annotations
The rest of the code gets the annotations that we talked about above easily using NLTK
```
df = pd.value_counts(np.array(all_words))

for i in range(len(df)-1):
  res_word = df.index[i]
  res_count = df.values[i]
  stem = word_stemmer.stem(df.index[i])
  lemme=wordnet_lemmatizer.lemmatize(df.index[i])
  pos_tag = nltk.pos_tag([df.index[i]])[0]
  
  if df.index[i] in wordsFiltered:
    mot_vide = 'oui'
  else:
    mot_vide = 'non'
  
  for chunk in nltk.ne_chunk(nltk.pos_tag([df.index[i]])):
    if hasattr(chunk, 'label'):
      named_entity = chunk.label()
    else:
      named_entity = '-'
  
  data.append([
        res_word,
        res_count,
        stem, 
        lemme,
        pos_tag[1], 
        mot_vide,
        named_entity
      ])
```



## Conclusion
We will write the result in a .csv file to see them well. Try running the code and see the result for yourself.
```
# write data in a file
with open('annotated_text.csv', 'w', encoding='UTF8', newline='') as f:
    writer = csv.writer(f)

    # write the header
    writer.writerow(header)

    # write the data
    for d in data:
      writer.writerow(d)
```

With that we finished getting the annotations for each word in our text, these annotations are important process of natural language processing as it helps us for tasks like "text auto-correct" or "text summary"...





