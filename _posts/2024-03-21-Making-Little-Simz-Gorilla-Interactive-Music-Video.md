Last summer I had the chance to intern at Pixel Lab, working on a project with Microsoft Azure and Little Simz! 
I had the chance to solve a really fun problem and wanted to write about what I did!

## The project
Microsoft Azure and Little Simz wanted to create an interactive music video using Microsoft AI, where the user can guide the video effects by clicking on song lyrics that resonate with them.

As the video plays, the user will be able to click on whatever lyrics they like. The AI will analyze the mood of these selected lyrics, and use that to guide the video effects.

I was tasked with creating a system that takes the user selection of lyrics and uses data from AI analysis to select the best matching effect.
I was responsible for designing this system from the ground up.

Little Simz’s team gave us an assortment of backgrounds, along with a green screen foreground video of Simz performing. We decided that effects should be applied to the background, foreground, or both.

## Here's how we did it

### Scoring based on mood/theme
**Identify some overarching themes**

I used ChatGPT to identify around ten to fifteen themes and moods from the song. I input Simz's lyrics and verbal descriptions of her body language and the backing track, and ChatGPT returned the most prominent themes/moods explored by the song.
**Match Themes to Lyrics**

I used ChatGPT to split the song into three lengths of chunks: 1/2-1 line, 1-3 lines, and 3-5 lines (roughly). For each of these chunks, ChatGPT came up with a score for each of the themes. Think of this as giving each chunk a "mood vector," that represents how much the chunk relates to each of the 10 themes.
The problem is that when a user clicks on a lyric chunk, they probably don't mean that they like those specific ~3 words — what they probably mean is that they relate to this section of the song, taking into account the lyrics around them. So, you can't just consider the "mood vector" of the specific few words they click on. Somehow, you need to consider the general area of the song as well!
So, for every word index, you can combine the overlapping small, medium, and large chunks to get a good sense of the dominant themes of that particular word! This gives a broader context to the selected lyric, while still assessing the lyrics near neighbors. It strikes a balance between too much context and too little context when determining themes for any given word.

**Video effect selection pool**

Using ShaderToy, I came up with a large bank of potential video effects. However, multiple effects are to be applied simultaneously — some work well together while others don’t. Considering all of the possible video effect combinations, I removed the pairs that didn't work well together.

In the end, we decided that background scenes (plates) and color tints should be kept separate from these effect combinations. This means that as the video plays, the matching algorithm selects an effect combo, plate, and color tint (or lack thereof), all separately. 

**Match Themes to Video Options**

Once all of these effect combos, plates, and color tints were identified, we generated verbal descriptions for them — a sentence that records what the effect looks like, what the plate looks like, and what color tint is being applied. 

Now, following similar methods to lyric-theme matching, we scored these video options against the previously identified themes.
Now, each video effect pair _also_ has a “mood vector,” (an array of scores corresponding to how much it relates with each of the themes) just like the lyric chunks!

Now the hard work is done! Each lyric has a scorecard that determines how much it relates to each of the moods/themes, and each video effect combination has the same scorecard.
What's better is that we can think of these scorecards as vectors, and do some linear algebra to determine how "similar" they are!

### Combining preferences with the default mood
**“Combining” user preferences with the current lyric**

**Some things to consider:**

- When a user selects a lyric, they’re suggesting that they like a particular theme, not that they dislike any other theme.
- When a user suggests that they like a particular theme, the video should be pushed in that direction, not completely changed to only show effects related to that theme. If theme A is being selected, but theme A is not at all present at this point in the song, theme A video effects shouldn’t be shown.
- When the user doesn’t click on anything at all, the video should reflect the themes present in the video with no bias towards any one theme. 

**So, here's a solution:**

As the users select lyric chunks, we need to somehow aggregate all of these lyrics scores to generate a "user preference card."
Once we have that user preference scorecard, we combine it with the baseline lyric card for the current index (remember, each word index has its _own_ theme card)!

As the video plays, the mood score of the current lyric's index is _combined_ with the user preference card! This way, user preferences "push" the effects towards their desired mood/theme preference, without completely hijacking the mood that the current lyrics represent!

Remember that the selected themes should only indicate what the user _is_ interested in, not what the user _isn’t_ interested in. So, to generate the user preference theme card from the selected lyrics, we only average the two arrays when the resulting index is greater than the original baseline lyric index. This makes it so that the user's lack of interest in a theme won’t lower the presence of that theme in the final card. 

In other words, we have this "baseline score," the theme scores we generated for each lyric index. As the video plays, we only want the users to be able to _boost_ a particular theme. We never want to _reduce_ a particular theme from being represented. Therefore, as the video plays and we need to combine the baseline current index score with the user preference card, we only combine the two scores if it would _increase_ the theme. We'll never _decrease_ the baseline lyric index score.

Therefore, if the user never selects anything, the final scorecard is simply the baseline score from the current lyric index.

### Matching to an effect
We now have a scorecard, a list of 10 numbers that represent the current theme/mood taking into account both the user preferences and the mood/theme of the current phrase.
What's left is to find the _video effect_ that best matches this scorecard!

We already scored all of the video effect combinations against the themes used in the user preference card!

I like to imagine that there's this 10-dimensional "mood space," where each dimension represents a particular mood. We've plotted every possible video effect combination in this 10-d mood space, and we also have a point representing the user's score card! All you have to do is find the video effect combination that's closest to the user's score card! This is why thinking of the mood scores as _mood vectors_ is helpful. We just take the Euclidian distance between the user score card and each effect pair, and return whatever effect combination that's the closest!

This effect combination (background plat, color tint, and video effect) is then applied to the video, and voila! We're done!
