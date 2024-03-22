Last summer I had the chance to intern at pixel lab, working on a project with Microsof Azure and Little Simz! 
I had the chance to solve a really fun problem and wanted to write about what I did!

## Project
Microsoft Azure and Little Simz wanted to create an interactive music video using Microsoft AI, where the user can guide the video effects by clicking on song lyrics that resonate with them.

As the video plays, the user will be able to click on whatever lyrics they like. The AI will analyze the mood of these selected lyrics, and use that to guide the video effects.

I was tasked with creating a system that takes the user selection of lyrics and uses data from AI analysis to select the best matching effect.
I was responsible for designing this system from the ground up.

Little Simz’s team gave us an assortment of backgrounds, along with a green screen foreground video of Simz performing. We decided that effects should be applied to the background, foreground, or both.

Determining Video Effects Given Lyric Selections
Identify some overarching themes
I used ChatGPT to identify around ten to fifteen themes and moods from the song. I input Simz lyrics and verbal descriptions of her body language and the backing track.
Match Themes to Lyrics
I used ChatGPT to split the song into three lengths of chunks: 1/2-1 line, 1-3 lines, and 3-5 lines (roughly). For each of these chunks, ChatGPT came up with a score for each of the themes.
Now, for every given word index, you can combine the overlapping small, medium, and large chunks to get a good sense of the dominant themes. This strategy gives a broader context to the lyric while still assessing the lyrics near neighbors. It strikes a balance between too much context and too little context when determining themes for any given word.
Video effect selection pool
Using ShaderToy, I came up with a large bank of potential video effects. However, multiple effects are to be applied simultaneously, and some work well together while others don’t. Looking at the potential combos, I specified which effects could be paired together, and which couldn’t.

In the end, we decided that background scenes (plates) and color tint should be kept separate from these effect combinations. This means that as the video plays, the algorithm selects the best fitting effect combo, the best fitting plate, and the best fitting color tint (or lack thereof) separately. 
Match Themes to Video Options
Once all of these effect combos, plates, and color tints were identified and given verbal descriptions, following similar methods to the lyric-theme matching, we scored these video options against the previously identified themes.
Now, each lyric index and video option has a “score card,” an array of integers corresponding to its rating on how well it relates to each of the themes.
Video Option Selection Pipeline
General strategy
As the video plays, we can get the current lyric’s theme score card.
Once we have this score card, we’ll find the “most similar” video effect.
As the user selects lyrics, we can “combine” their preferences with the current lyric to find the most similar video effect.
“Combining” user preferences and current lyric card
Some things to consider:
When a user selects a lyric, they’re suggesting that they like a particular theme, not that they dislike any other theme.
When a user suggests that they like a particular theme, the video should be pushed in that direction, not completely changed to only show effects related to that theme. If theme A is being selected, but theme A is not at all present at this point in the song, theme A video effects shouldn’t be shown.
When the user doesn’t click on anything at all, the video should reflect the themes present in the video with no bias towards any one theme. 
Instead of averaging all of the users selected lyrics, we first reduce the score cards noise by only including a few of the highest values. Ex. [2,3,7] turns into [0, 0, 7]. Then, we build a new array containing only the maximums of each of the selected array indexes.
Now that we have a filtered selected lyric score card, we combine it with the baseline lyric card for the current index. Remember that these themes only indicate what the user is interested in, not what the user isn’t interested in. 
So, to get the final theme preference card, we only average the two arrays when the resulting index would be greater than the original baseline lyric’s index. This makes it so that the users lack of interest in a theme won’t lower the presence of that theme in the final card.
And of course, if there are no selected lyrics, the final score card is simply the score from of the current lyric index.
Matching final theme card to video options
The following process is used to find the best fit effect combo, background (plate), and color tint (or lack thereof).
First, we look at the top two dominant themes in the final theme card. These are the themes we want to prioritize. So, we filter through our video options bank so that we’re only looking at video options that rank at least as strongly in at least one of these themes.
Now, given the final theme card and the filtered video options theme cards, the video option with the smallest euclidian distance is the option to be applied!
