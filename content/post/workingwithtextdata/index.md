+++
title = "Working With Text Data"

date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false

# Authors
authors = ["Michael W. Brady"]
+++
## Tokenizing Text

A token is "an instance of a sequence of characters in some particular document that are grouped together as a useful semantic unit for processing." ([Intro to Information Retrieval](https://nlp.stanford.edu/IR-book/))

Attributes of good tokens:

- **Iterable Data Structure:** Tokens should be stored in an iterable data structure to allow analysis of the semantic unit (e.g., list, generator)
    - Split multi-word strings with `.split(' ')`
- **Lower Case:** Tokens should all be in the same case (lower is typically easier to read)
    - Use `.lower()`
- **Free of non-alphanumeric chars:** Tokens should have punctuation, whitespace, etc. stripped
    - Use: `re.sub(r'[^a-zA-Z ^0-9]', '')`

**Custom Tokenization:**

    def tokenize(text):
        """Parses a string into a list of semantic units (words)
    
        Args:
            text (str): The string that the function will tokenize.
    
        Returns:
            list: tokens parsed out by the mechanics of your choice
        """
        text = text.lower()
        tokens = re.sub(r'[^a-zA-Z ^0-9]', '', text)
        tokens = tokens.split(' ')
        
        return tokens

### SpaCy Tokenization:

    import spacy
    from spacy.tokenizer import Tokenizer
    
    nlp = spacy.load("en_core_web_md")
    
    # Tokenizer
    tokenizer = Tokenizer(nlp.vocab)
    
    tokens = []
    
    """ Make them tokens """
    for doc in tokenizer.pipe(df['reviews.text'], batch_size=500):
        doc_tokens = [token.text for token in doc]
        tokens.append(doc_tokens)
        
    df['tokens'] = tokens

### Visualizing The Most Common Words

    import squarify
    import matplotlib.pyplot as plt
    
    wc_top20 = wc[wc['rank'] <= 20]
    
    squarify.plot(sizes=wc_top20['pct_total'], label=wc_top20['word'], alpha=.8 )
    plt.axis('off')
    plt.show()

## Statistical Trimming

Refers to the concept of removing the most frequent and least frequent words from the overall corpus. This makes sense because:

> The words that appear most frequently may not provide any insight into the meaning on the documents since they are so prevalent.

Words that appear infrequently (at the end of the graph) also probably do not add much value, because the are mentioned so rarely.

## Stop Words

Stop words are common words that do little to differentiate a string from another string (because they are common and on their own do not mean much). Most libraries come with built in stop word vocabularies. Removing stop words can be relatively easy: 

    nlp = spacy.load("en_core_web_sm")
    
    # Add custom stop words
    STOP_WORDS = nlp.Defaults.stop_words.union(['I','we'])
    
    updated_tokens = []
    
    for tokens in df['tokens']:
        real_tokens = []
        for token in tokens:
            if token not in STOP_WORDS:
                real_tokens.append(token)
        updated_tokens.append(real_tokens)
        
    df['tokens'] = updated_tokens

## Stemming

A process that removes common endings from words—it is used a part of the normalization process. SpaCy does not do stemming, thus it is necessary to use `nltk`

    stems = []
    for token in df['tokens']:
        token_stems = []
        for word in token:
            word = ps.stem(word)
            token_stems.append(word)
        stems.append(token_stems)
        
    df['stems'] = stems

Stemming works in applications where humans don't have to worry about reading the results.

## Lemmatization

Lemmatization is similar to stemming but is more methodical seeking to transform words to their base form (e.g., started —> start). SpaCy implements an efficient lemmatization method: 

    def get_lemmas(text):
        lemmas = []
        doc = nlp(text)
        
        for token in doc:
                lemmas.append(token.lemma_)
        return lemmas
    
    df['lemmas'] = df['reviews.text'].apply(get_lemmas)

### Useful String Methods

Clean irregular whitespace from a string: 

    " ".join(text_string.split())

Formatting printing of floats:

    f'On Tuesday the temperate was {temperature:.0f}'

Split string by separator

    .split(<sep>)

Split string by separator and retain separator

    .partition(<sep>)

## Regex

    # matchObject = re.search(pattern, input_str)
    
    # Search for the first lowercase letter
    regex = r"[a-z]" 
    match = re.search(regex, "June 24")
    
    # Look at the match object
    print(match)
    
    # Look at the matching string
    print(match[0])
    
    # Look at slice indices and grab the match manually
    print("Slice indices for this string:", match.start(), match.end())
    print("June 24"[match.start():match.end()])

[Python Regular Expression Cheatsheet - Debuggex](https://www.debuggex.com/cheatsheet/regex/python)

[Regex Cheat Sheet: A Quick Guide to Regular Expressions in Python](https://www.dataquest.io/blog/regex-cheatsheet)

### Capturing matches into tuples:

    # Capture both month and day separately
    regex = r"([a-zA-Z]+) (\d+)" 
    
    string = "June 24, other thing, February 6, doesn't match, March 14"
    
    search_result = re.findall(regex, string)
    print(search_result)
    
    for match in search_result:
      print(match)
    
    # Convert to DataFrame easily
    df = pd.DataFrame(search_result, columns=['Month', 'Day'])
    df.head()

### Extract Text To New Column

    # with regex
    df['first name'] = df['full name'].str.extract('(\w+$)', expand=True)
    
    # Without regex
    df['first name'] = df['full name'].apply(lambda x: x.split(",")[0])

[https://www.youtube.com/watch?v=K8L6KVGG-7o&t=120s](https://www.youtube.com/watch?v=K8L6KVGG-7o&t=120s)

## Beautiful Soup

    from bs4 import BeautifulSoup
    import requests
    
    page = requests.get(page_to_scrape)
    
    # The "soup" is the parsed html of a webpage.
    soup = BeautifulSoup(page.text, 'html.parser')
    
    # Print contents of page (may be very large)
    print(soup.prettify())
    
    # .find() give everything within an html tag on the page
    nav_stuff = soup.find(id='globalNav')
    body_stuff = soup.find(class_='BodyText')
    
    # .find_all() returns all the specified elements on the page
    # returns a list 
    links = links_list.find_all('a')
    
    # Adding .text to a list element strips away the html elements
    for link in links:
    	print(link.text)
    
    # Convert to Pandas DataFrame
    
    clean_links = []
    for link in links:
    	clean_links.append(link.text)
    
    df = pd.DataFrame({'link_name':links})

Handy functions to strip html tags from strings: 

    from html.parser import HTMLParser
    
    class MLStripper(HTMLParser):
        def __init__(self):
            self.reset()
            self.strict = False
            self.convert_charrefs= True
            self.fed = []
        def handle_data(self, d):
            self.fed.append(d)
        def get_data(self):
            return ''.join(self.fed)
    
    def strip_tags(html):
        s = MLStripper()
        s.feed(html)
        return s.get_data()
    
    clean = strip_tags(str(text))

## Read the contents of a file

    with open('data.txt', 'r', encoding='utf-8') as f:
      contents = f.read()
      
    contents

## Streaming Documents with `yield`

    def doc_stream(path):
        for f in os.listdir(path):
            with open(os.path.join(path,f)) as t:
                text = t.read().strip('\n')
                tokens = tokenize(str(text))
                yield tokens