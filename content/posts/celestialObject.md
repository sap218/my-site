+++
title = "The Space Ontology and a Beginners Guide to Ontologies"
date = "2026-01-29"
categories = ['python','ontology','nlp','tf-idf','tool']
toc = true
+++

## CelestialObject: the Space Ontology and a Beginners Guide to Ontologies

Here you'll read an introduction to ontologies and I'll walk through a practical, end-to-end example of an ontology in practice.

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

Computational ontologies are machine readable with three fundamental components: classes, relationships, and metadata [3,4].

#### Classes

Classes [3] represent an entity within the domain and we can categorise these entities.
In our solar system we have many planets, but we can define this further by planet location within the solar system, for example we can have the high-level category "solar system" (superclass) with entities "inner system" and "outer system" (subclasses).
We then define planets if they are within the inner or outer system.

#### Relationships

Relationships link classes together, they describe how they are related, and provide deeper meanings to the domain [2,3].
Superclass and subclass relationships are natural relationships: `subclassOf`.
We can also explicitly define relationships, for example, the moon `orbits` the Earth, or Jupiter is a `instanceOf` gas giant. 
**Axioms** are logical relationships that are true without question and provide deeper knowledge within the domain [2,3]. For example:

`moon orbits earth -> earth orbits sun ->  ` then logically `moon orbits sun` [indirectly of course].

#### Metadata

The third and final concept is metadata, also known as "annotations".
Annotating ontology classes is adding more, valuable information and context for users and machines to better understand the class [3].
Classes can have a definition, for example, a comet: "formed of ice and dust with tails". 
A valuable annotation is database cross-reference (`dbXref`), which links a class to an external database, for example another ontology.
There are other metadata, like "seeAlso" which could be a link to an external resource, e.g. https://science.nasa.gov/solar-system/comets/ 

{{< alert type="info" >}}
Each concept in an ontology is uniquely identified with an IRI (Internationalized Resource Identifier); which we can use as cross-references [3,5].
{{< /alert >}}

Another important annotation is synonyms. Classes should be formal vocabulary [1,3,5] and synonyms should be included for terms we better know them as.
For example, the formal term for "the sun" should be "sol", furthermore we often refer to Earth as "the world" or "globe".
Synonyms can be exact, broader, or related terms, recently ontologies have been expanding to include lay terms [6].
Including all types of synonyms means we can bridge the gap between formal and standardised terms - perhaps used by experts - with everyday language.

### What does this all mean? 

My research interests are the voices of people: having their experiences heard and the terms they are using reflected in science.
To do this, I need a way to listen - this is where ✨ data ✨ is extremely useful! 
User-generated data - social media, questionnaires, forums, etc. - is hard to curate: unstructured, messy, and noisy.  

Once we have this data in a research-ready format, now we need to know how to extract the useful information. (all information is valuable, but conversations can go off topic)
This is where **ontologies** come into play! 
An ontology *should* contain all entities within a domain, I can use an ontology's classes and synonyms to extract the useful information from text.

I introduce the **Space Ontology** as a working scenario:

### Developing the Space Ontology

Creating an ontology means space entities will be in one place, reusable for others, and rich of metadata! Let's conceptualise space...

What sort of things are in space? We have the solar system: there is an inner and outer system.
Within the inner system, we have Mercury, Venus, Earth and Mars.
The outer system has Jupiter, Saturn, Uranus and Neptune. (and Pluto?)

{{< figure src="/images/posts/celestialobject/space.png" width=75% class="figure-plain" caption="**Figure**: A *drawing* of a few elements in our solar system." alt="electronic drawing of a few elements of our solar system" >}}

Planets have features, such as orbital ring systems. There are various space phenomena: solar flares and meteors.
And we know of satellites: artificial and natural ones. The moon is a natural satellite, whereas the Hubble Space Telescope is an artificial one.
Asteroids, comets, and dwarf planets!

#### Ontology foundation

Now I can start to **categorise**:

{{< figure src="/images/posts/celestialobject/concepts.png" width=90% class="figure-plain" caption="**Figure**: A simplified diagram of some elements of the solar system. Rectangles are superclasses, circles are subclasses, and diamonds represent relationships between classes." alt="a simplified diagram of some elements of the solar system" >}}

**Side note**: in this post, I won't be diving deep into relationship types and will stick to `subclassOf` types.

This is when I start designing a foundation of an ontology in a spreadsheet, see [`space.xlsx`](https://github.com/sap218/CelestialObject/tree/main/excel "link to space excel spreadsheet foundation").
I used `UFO` as the IRI.
Each tab in my spreadsheet is a superclass domain, then I create a column for class, another for subclass if necessary, and then the following columns are metadata.
Each row in a representation of a class.

The classes are formal terminology and I included various metadata, including synonyms.
For example: we have the sun, but I used `sol` as it is our star's name, definition "the centre of the solar system", synonyms "star; the sun", `dbXref` [`WD:Q525`](https://www.wikidata.org/wiki/Q525 "link to wiki data for the sun") and externalResource "https://science.nasa.gov/sun/".

With some **coding** experience, in `Python` I converted the `xlsx` into an `owl` ontology - for non-technical users, [`protégé`](https://protege.stanford.edu/ "protege website") is a free, open-source ontology editor.

**What is an ontology without a project?**

With a dataset of social media posts of users talking about space, I can see some interesting discussions:
```
UserA: The Sun is extra bright today!
UserB: its a solar eclipse! you shouldnt look at it!
UserA: I forgot sunglasses...
UserC: NO NO even if you forgot sunglasses you shouldn't look at the sun directly!
```
In this snippet, we can see users talking about a solar eclipse and that someone forgot their sunglasses...thus line 3 is probably not relevant to the solar system project.

To analyse this data, I need a way mine the lines that are most relevant - classes from the space ontology helps us here.
And although I know some words relevant to space, I may be missing important classes, or synonyms.

The full dataset is here: [`social_media_posts.txt`](https://github.com/sap218/CelestialObject/tree/main/corpus "social media data location"). (I invented these fake conversations)

#### Topics of interest

We can extract classes and corresponding metadata from ontologies, e.g. with these classes:
```
mars
sol
```
We will have these synonyms:
```
mars
red planet
sol
star
the sun
```

#### Annotating text

I can now use these terms from the ontology to filter the social media data. Including synonyms with classes means we *should* end up with more relevant text in the results.

{{< figure src="/images/posts/celestialobject/wordcloud.png" width=80% class="figure-plain" caption="**Figure**: A wordcloud of concepts from the data, sized by frequency." alt="wordcloud of concepts from the data" >}}

BUT - as mentioned, I may be missing important classes, or synonyms.
I *could* go back to the ontology development stage - OR I could analyse the text data...

#### New terms

After removing ontology concepts from the text data, I can run a statistical techniques that ranks and highlight terms of "importance" in context of document frequency: 

{{< figure src="/images/posts/celestialobject/ranked_plot.png" width=70% class="figure-plain" caption="**Figure**: Bar plot of Term Frequency-Inverse Document Frequency (TF-IDF) on the data, ordered by importance." alt="bar plot of term frequency inverse document frequency on the data" >}}

From the above Figure, we can see some terms are ranked high in "importance" but are not relevant to the solar system (e.g. "year" and "time").
Although we can acknowledge that these are important for the users. (that's a different project...)
 
Most notably, **eclipse** is in the bar plot and I did not previously consider this!

#### Updating the ontology

With my new discoveries, I update the ontology with more coding magic!

Below is the final version of the ontology: with eclipse present at the bottom `subclassOf` space phenomena.

{{< figure src="/images/posts/celestialobject/web-plot.png" width=95% class="figure-plain" caption="**Figure**: Web plot of the final Space Ontology, orange nodes representing superclass domains, and blue nodes classes/subclasses." alt="web plot of the final space ontology" >}}

#### Rerunning steps for a better outcome...

I can do step (2) again: re-extract the classes and synonyms from the ontology and now the list is bigger as it'll include the new concepts (eclipse and corresponding synonyms).
Then I rerun step (3) to annotate the text data: meaning more data output, thus a more fruitful investigation!

### Conclusion

Ontologies represent a *thing* and contribute to the semantic web.
Being open source means they are reusable, and they can be developed together.

Metadata is powerful for text annotation: using only formal class labels can limit outputs, but with synonyms of a variety - spanning formal, technical, and natural/everyday language - we can source more data.
All of these terms in ontologies can bridge formal terminologies to lay vocabulary.

Here I encapsulated some elements of Space within an ontology and presented a working example.
The methodologies are reusable beyond space! 🚀

#### Important links

+ GitHub repository for the Space Ontology, test files, and plots, [**CelestialObject**](https://github.com/sap218/CelestialObject "github repository for space ontology").

+ As I mentioned, I have ✨ coding ✨ experience but many software tools for text tasks are not designed to consider ontologies. So I made my code usable! [Jabberwocky](https://github.com/sap218/jabberwocky "jabberwocky github repository") allows users to conduct various text-related tasks whilst easily manipulating ontologies [7].

### Citations

1. 	Gruber TR. Toward principles for the design of ontologies used for knowledge sharing? International Journal of Human-Computer Studies. 1995;43: 907–928.
2. 	Stuart R, Peter N, Others. Artificial intelligence: a modern approach. Upper Saddle River, NJ: Prentice Hall, USA; 2003.
3. 	Hoehndorf R, Schofield PN, Gkoutos GV. The role of ontologies in biological and biomedical research: a functional perspective. Briefings in Bioinformatics. 2015;16: 1069–1080.
4. 	Haendel MA, Chute CG, Robinson PN. Classification, Ontology, and Precision Medicine. The New England Journal of Medicine. 2018;379: 1452–1462.
5. 	Leonelli S. Bio-ontologies as Tools for Integration in Biology. Biological Theory. 2008;3: 7–11.
6. 	Vasilevsky N, Engelstad M, Foster E, Mungall C, Robinson PN, Köhler S, et al. Enhancing the Human Phenotype Ontology for Use by the Layperson. 9th International Biocuration Conference: ICBO/BioCreative. F1000Research; 2016. doi:10.7490/f1000research.1111752.1
7. 	Pendleton S, Gkoutos G. Jabberwocky: an ontology-aware toolkit for manipulating text. Journal of Open Source Software. 2020;5: 2168.
