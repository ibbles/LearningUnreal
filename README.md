# Learning Unreal
A bunch of notes and code snippets that I write while figuring out how to use the Unreal Engine.

These notes have been written as part of evaluation moving from Zettlr to Tangent Notes and Obsidian for my note taking.
The old Zettlr notes are in the [LearningUnrealEngine](https://github.com/ibbles/LearningUnrealEngine) GitHub repository.
If you find these notes useful then perhaps you will find something useful there as well.
I have not yet moved all the notes over since I don't yet know which format I'll be using long-term once the Tangent Notes / Obsidian evaluation is over.

I use a Zettelkasten-like method for my note taking.
Notes are typically small-ish but not atomic.
I write in short sentences.
This is not a book so the flow of text is not important.
The intended audience is not a new person that would read these notes from start to finish.
The notes are for me, not for you.
The intention is to be able to read any sentence and get something from it.

I structure my notes and links in a way that makes the Obsidian Graph View readable.
I do this by only creating strong links (``[[Link Target]]`` ) for related notes.
I do not create strong links to good-to-know information.
I use weak links (`[Link Target](Link Target.md)`) for good-to-know information.
Prerequisites, if you will, for the current note.
Weak links do not appear in the Graph View, while strong links do.
(
Turns out I was wrong, weak links do show up in the Graph View. Dammit.
Now I don't know what to do.
Should I link to my heart's content and get a very dense graph?
That would mean giving up on any deliberate graph structure.
Which is kind of the point, the graph structure should emerge from the content.
But I don't think the node layout algorithm is smart enough to expose that structure.
It sure hasn't for the small amount of notes and links I've collected so far.
It seems to need a lot of help.
Which was the purpose of the tag-rooted clusters.
Should I link less? And mark weak links with some formatting that Obsidian doesn't know is a link?
That feels like working against the tool.
For now, I'm going to just create links whenever I feel there should be one.
I need rules to base these feelings on.

Rules for linking:
- Prefer "up-links", i.e. more specific topics link to the general concept they are part of.
- Prefer "down-links", i.e. general topics link to more specific use cases.
- Which one of the two above should I advocate for?

What is the purpose of a link?
- To group related things, making pattern emerge implicitly over time.
- To aid reading, letting the user jump from note to note in the search for knowledge.

The primary reason for my note taking is the latter of the two.
The former helps with finding information among the notes.
The reason why I switched from Zettlr to Tangent and then to Obsidian was to get the overview that the Graph View provides.
If I create too many links then I fear the Graph View will lose its capacity for giving an overview.
But maybe it doesn't. Maybe it handles many links just fine. Maybe I worry over nothing.

OK. Attempt 1. Create a link whenever I believe a reader of a particular note might have used of the information in another note.
Even when the two notes are in separate tag-rooted clusters.
To hold back a little bit, let's not create links just for grouping.
Groups should emerge anyway.
I still think the the onion layers approach to linking hierarchy is a good idea.
By this I mean that each note should live in some layer in a tag-rooted cluster and though should be put into which nodes it should be sandwiched between.
)

I use tags as entry points, not for grouping/listing.
One cannot filter on a tag and expect to find all notes dealing with that topic.
I use tags as the center of a cluster of notes in the Graph View and then layer notes with more and more specific topics around them.
I call such a collection of notes a tag-rooted cluster.

I would like to know how to do far links in Obsidian.
A far link would link from a note in one tag-rooted cluster to a note in another cluster.
Far links would have a much smaller link force in the Graph View than strong links.

Notes have very few tags, most often 1 and only rarely 2. 3 is very uncommon.
