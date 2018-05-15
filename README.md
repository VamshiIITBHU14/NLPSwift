**Natural-language processing (NLP):**

Wikipedia says, Natural-language processing (NLP) is an area of computer science and artificial intelligence concerned with the interactions between computers and human (natural) languages, in particular how to program computers to fruitfully process large amounts of natural language data. Challenges in natural-language processing frequently involve speech recognition, natural-language understanding, and natural-language generation.


**NLP in iOS with NSLinguisticTagger:**

NSLinguisticTagger provides a uniform interface to a variety of natural language processing functionality with support for many different languages and scripts. One can use this class to segment natural language text into paragraphs , sentences, or words and tag information  about those segments such as parts of speech, lexical class, lemma, script and language.

**Part of speech (POS):**

In English language, noun, verb, adjective, adverb, pronoun , preposition , conjunction and interjection are part of speech.

**Lexical class:**

Lexical class is same as POS but may exclude parts of speech that are considered to be functional, like pronoun

**Identify Names:**

Names of person or places

**Perform Lemmatization:**

The process of grouping together the inflected forms of a word so that they can be analysed as a single item, identified by the word's lemma. Eg: go, goes, gone, went are lemma of go.

**Determine the language:**

Return the dominant language for the specified string

**Use cases for NSLinguisticTagger: **

1) Voice driven search - Parse and lemmatize the search command. Define grammar to map the spoken search command into an actual search query

2) To automatically create or suggest tags found in a document such as recongizing proper nouns, organisation names.

3) Facilitate conversational UI.


