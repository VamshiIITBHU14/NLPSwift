
**EDIT as per 02, June, 2018 (IST)**

Whole of the README translated to Russian by @Gargo. Please find it hosted here. 
http://gargo.of.by/nlpswift/

**Natural-language processing (NLP):**

Wikipedia says, Natural-language processing (NLP) is an area of computer science and artificial intelligence concerned with the interactions between computers and human (natural) languages, in particular how to program computers to fruitfully process large amounts of natural language data. Challenges in natural-language processing frequently involve speech recognition, natural-language understanding, and natural-language generation.


**NLP in iOS with NSLinguisticTagger:**

NSLinguisticTagger provides a uniform interface to a variety of natural language processing functionality with support for many different languages and scripts. One can use this class to segment natural language text into paragraphs , sentences, or words and tag information  about those segments such as parts of speech, lexical class, lemma, script and language.



***Coding Example :***

```
import UIKit

let inputString = "In Old Delhi, a neighborhood dating to the 1600s, stands the imposing Mughal-era Red Fort."


// tag schemes: tag schemes are constants that are used to identify pieces of information that we want from the input text. Tag schemes asks tagger to look for informations like
// Token type: a contant to classify each character as a word, punctuation or a whitespace
// Language: a constant to determine langugage of the token
// LexicalClass: this constant determines class of each token. i.e. it determines part of speech for a word, type of punctuation for a punctuation or type of whitespace for a whitespace
// Name type: this constant looks for tokens that are part of a named entity. It will look for a person's name , organizational name and name of a place
// Lemma: this constant returns the stem of word.
let tagger = NSLinguisticTagger(tagSchemes: [NSLinguisticTagScheme.tokenType, .language, .lexicalClass, .nameType, .lemma], options: 0)

// Options are the way to tell API as how to split the text. We are asking to ignore any punctuations and any whitespaces. Also, if there is a named entity then join it together i.e instead of considering "New" "Delhi" as two entities, join them together as one which is "New Delhi"
let options: NSLinguisticTagger.Options = [NSLinguisticTagger.Options.omitPunctuation, .omitWhitespace, .joinNames]
```
**Part of speech (POS):**

In English language, noun, verb, adjective, adverb, pronoun , preposition , conjunction and interjection are part of speech.

```
func partOfSpeech() {
    tagger.string = inputString
    let range = NSRange(location: 0, length: inputString.utf16.count)
    
    tagger.enumerateTags(in: range, unit: NSLinguisticTaggerUnit.word, scheme: NSLinguisticTagScheme.lexicalClass, options: options) { (tag, tokenRange, _) in
        if let tag = tag {
            let word = (inputString as NSString).substring(with: tokenRange)
            print("\(tag.rawValue) -> \(word)")
        }
    }
}
```

Calling the method in Playground gives the following response: 

```
partOfSpeech()

Preposition -> In
Noun -> Old Delhi
Determiner -> a
Noun -> neighborhood
Verb -> dating
Preposition -> to
Determiner -> the
Number -> 1600s
Verb -> stands
Determiner -> the
Adjective -> imposing
Noun -> Mughal
Noun -> era
Noun -> Red
Noun -> Fort
```


**Lexical class:**

Lexical class is same as POS but may exclude parts of speech that are considered to be functional, like pronoun

**Identify Names:**

Names of person or places

```
func namedEntity(str: String) {
    tagger.string = str
    
    let range = NSRange(location: 0, length: str.utf16.count)
    
    let tags: [NSLinguisticTag] = [NSLinguisticTag.personalName, .placeName, .organizationName]
    
    tagger.enumerateTags(in: range, unit: NSLinguisticTaggerUnit.word, scheme: NSLinguisticTagScheme.nameType, options: options) { (tag, tokenRange, _) in
        
        if let tag = tag, tags.contains(tag) {
            let name = (str as NSString).substring(with: tokenRange)
            print("\(name) : \(tag.rawValue)")
        }
        
    }
}

namedEntity(str: inputString)
```

Output:

```
Old Delhi : PlaceName

```

**Perform Lemmatization:**

Lemmatization is the process of breaking down word into its most basic form. For example, "go" can be transformed into "gone", "will go", "went" etc and since therere are so many forms for NLP to understand it better the words are converted into their base root, like in this case all the above forms transform to "go" and this "go" is known as Lemma

```
func lemmatizeString() {
    tagger.string = inputString
    
    let range = NSRange(location: 0, length: inputString.utf16.count)
    
    tagger.enumerateTags(in: range, unit: NSLinguisticTaggerUnit.word, scheme: NSLinguisticTagScheme.lemma, options: options) { (tag, tokenRange, _) in
        if let lemma = tag?.rawValue {
            print(lemma)
        }
    }
}

lemmatizeString()
```

Output:
```
in
a
date
to
the
stand
the
imposing
era
red
fort
```

**Determine the language:**

Return the dominant language for the specified string.

```
// Language identification
// here we will give input string to the tagger and ask the dominant language in the string.
// Dominant language is the most frequently occurring language in the string so if our string had mix of english, hindi, spanish and french words then it would choose the most common language

func languageIdentification() {
    tagger.string = inputString
    print(tagger.dominantLanguage!)
}
```

Calling the method gives output as :

``` 
languageIdentification()

en

```

**Use cases for NSLinguisticTagger:**

1) Voice driven search - Parse and lemmatize the search command. Define grammar to map the spoken search command into an actual search query

2) To automatically create or suggest tags found in a document such as recongizing proper nouns, organisation names.

3) Facilitate conversational UI.


