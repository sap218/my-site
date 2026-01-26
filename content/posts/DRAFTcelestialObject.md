+++
title = "The Space Ontology and a Beginners Guide to Ontologies"
date = "2026-01-26"
categories = ['python','ontology','nlp','tf-idf','tool']
toc = true
draft = true
+++

## CelestialObject: the Space Ontology and a Beginners Guide to Ontologies

Ontologies represent a domain of knowledge: a **thing** [1].
This *thing* could be **space**: our solar system, planets, satellites, and other celestial objects.
It is a map of a domain of what is true within a *thing* and how entities relate to each other.

The idea is to categorise and structure information in both a readable and understandable manner, whilst also standardising the vocabulary.

{{< alert type="info" >}}
Categorisation dates back to 400 B.C. when philosopher Aristotle, expressed the importance of classification, introducing the notion of "species" for categorisation: humans, cats, owls.
{{< /alert >}}

### Computational Ontology

Computational ontologies were explored in the 20th century with computational advancements [2].
Computational ontologies are now readable by both the user and the machine: building upon the "semantic web": the network of information.

In semantics, we logically think about how closely things are related to one another, this can be entities or words.
For example, the moon is more closely related to planets as both are solid celestial objects, whereas the sun is a massive ball of gas.
Moreover, in terms of words, we can say that "world" is semantically related to "planet".

Computational ontologies are machine readable with three fundamental entities: classes, relationships, and metadata [3,4].

Let's conceptualise **space** as an ontology...
Our solar system: we have the sun, our planet, the moon. There are other planets in the solar system and celestial objects, like comets and asteroid belts.

[image]

#### Classes

Classes [3] represent an entity within the domain and we can categorise these entities.
In our solar system we have many planets, but we can define this further by planet location within the solar system, for example we can have the high-level category "solar system" (superclass) with entities "inner system" and "outer system" (subclasses).
We then define planets if they are within the inner or outer system.

#### Relationships

Relationships link classes together, they describe how they are related, and provide deeper meanings to the domain [2,3].
Superclass and subclass relationships represent natural relationships.
We can also explicitly define relationships, for example, the moon "orbits" the Earth, or Jupiter is a "typeOf" gas giant.

Axioms are logical relationships that are true without question and provide deeper knowledge within the domain [2,3].
For example: class A subclass of class B and class B subclass of class C, then we can logically infer that class A subclass of class C. 

+ Moon "orbits" Earth;
+ Earth "orbits" sun;
+ Logically the moon orbits the sun [indirectly of course].

[image]

#### Metadata

The third and final concept is metadata, also known as "annotations".
Annotating ontology classes is adding more, valuable information and context for users and machines to better understand the class [3].
Classes can have a definition, for example, a comet: "formed of ice and dust with tails". 
A valuable annotation is database cross-reference (`DbXref`), which links a class to an external database, for example another ontology.
There are other metadata, like "seeAlso" which could be a link to an external resource, e.g. https://science.nasa.gov/solar-system/comets/ 

{{< alert type="info" >}}
Each concept in an ontology is uniquely identified with an IRI (Internationalized Resource Identifier); which we can use as the DbXrefs [3,5].
{{< /alert >}}

Another important annotation is synonyms. Classes should be formal vocabulary [1,3,5] and synonyms should be included for terms we better know them as.
For example, the format term for "the sun" should be "sol", furthermore we often refer to Earth as "the world" or "globe".
Synonyms can be exact, broader, or related terms, recently ontologies have been expanding to include lay terms [6].
Including all types of synonyms means we can bridge the gap between formal and standardised terms - perhaps used by experts - with everyday language.

### What does this all mean? A scenario...

My research interests are the voices of people: having their medical experiences heard, their opinions listened to, and the terms they are using reflected in science.
I aim to bridge the gaps between clinical terminology and the patient's vocabulary.

To listen to users, we need access to user-generated data. This type of data is usually unstructured and messy, such as social media posts full of emojis. 
But this data can be mined, cleaned, and processed into a research-ready format.

**As an example, I introduce the Space Ontology:**

#### Developing the Space Ontology

I started with some social media posts of users talking about space, see [`social_media_posts.txt`](https://github.com/sap218/CelestialObject/tree/main/corpus). 
But as we know, people can use social media to discuss things off-topic: (in this test data, I've invented fake conversations)

```
UserA: The Sun is extra bright today!
UserB: its a solar eclipse! you shouldnt look at it!
UserA: I forgot sunglasses...
UserC: NO NO even if you forgot sunglasses you shouldn't look at the sun directly!
```
In this example, sunglasses could've turned the conversation into a different one.
And so I need a way to mine the text to only include relevant information. This relevant information can come from an ontology...

##### Ontology foundation

Although I know some words relevant to space, I may be missing important concepts, or synonyms.
This is when I start designing a foundation of an ontology in a spreadsheet, see [`space.xlsx`](https://github.com/sap218/CelestialObject/tree/main/excel).
I used `UFO` as the IRI.

With some ✨ coding ✨ experience, I can convert into an ontology - for non-technical users, [`protégé`](https://protege.stanford.edu/) is a free, open-source ontology editor.

##### Topics of interest

Despite my ontology designed, it turns out I only have a few topics of interest, e.g. specifically planets:

```
mars
saturn
```

I can now use these terms from the ontology to filter the social media data.
But with ontology metadata, I know that Mars has the synonym "red planet" so my results should hopefully have more data as I'm including synonyms.

##### Annotating text

With these classes and corresponding synonyms, and some ✨ coding ✨ experience, I can now annotate the social media data for my terms of interest!
BUT...I've realised that I may have missed out some synonyms from my ontology building stage...

[figure - wordcloud]

##### New terms

With more ✨ coding ✨ experience, I can conduct some statistical techniques on the text data to rank and highlight terms of "importance" in context of document frequency. 
I've now discovered classes and synonyms I didn't previously consider...

[figure - rank terms]

##### Updating the ontology

With my new discoveries, I update the ontology!

##### Rerunning (2) and (3)

I can do step (2) again for more terms for my topics of interest, and rerun step (3) to annotate the text data: meaning a more fruitful output for my investigations!

[figure - ontology plot]

[figure cyannnotator]

#### Important links

+ GitHub repository for the Space Ontology and test files, [**CelestialObject**](https://github.com/sap218/CelestialObject "github repository for space ontology").

+ As I mentioned, I have ✨ coding ✨ experience but many software tools for text tasks are not designed to consider ontologies. So I made my code usable! [Jabberwocky](https://github.com/sap218/jabberwocky "jabberwocky github repository") allows users to conduct various text-related tasks whilst easily manipulating ontologies [7].

### Citations

1. 	Gruber TR. Toward principles for the design of ontologies used for knowledge sharing? International Journal of Human-Computer Studies. 1995;43: 907–928. doi:10.1006/ijhc.1995.1081
2. 	Stuart R, Peter N, Others. Artificial intelligence: a modern approach. Upper Saddle River, NJ: Prentice Hall, USA; 2003. Available: https://zoo.cs.yale.edu/classes/cs470/materials/aima2010.pdf
3. 	Hoehndorf R, Schofield PN, Gkoutos GV. The role of ontologies in biological and biomedical research: a functional perspective. Briefings in Bioinformatics. 2015;16: 1069–1080. doi:10.1093/bib/bbv011
4. 	Haendel MA, Chute CG, Robinson PN. Classification, Ontology, and Precision Medicine. The New England Journal of Medicine. 2018;379: 1452–1462. doi:10.1056/NEJMra1615014
5. 	Leonelli S. Bio-ontologies as Tools for Integration in Biology. Biological Theory. 2008;3: 7–11. doi:10.1162/biot.2008.3.1.7
6. 	Vasilevsky N, Engelstad M, Foster E, Mungall C, Robinson PN, Köhler S, et al. Enhancing the Human Phenotype Ontology for Use by the Layperson. 9th International Biocuration Conference: ICBO/BioCreative. F1000Research; 2016. doi:10.7490/f1000research.1111752.1
7. 	Pendleton S, Gkoutos G. Jabberwocky: an ontology-aware toolkit for manipulating text. Journal of Open Source Software. 2020;5: 2168. doi:10.21105/joss.02168
