[Blog Post](https://towardsdatascience.com/using-network-science-to-explore-hashtag-culture-on-instagram-1f7917078e0)

There are two Python files here, each containing a custom Instagram class.

You will need to have an Instagram account to use InstagramScraper()

# InstagramScraper()

This class is made up of a series of methods that allow for the scraping of Instagram post data. The pipeline consists of three main methods that need to be called sequentially.  There is no current method to chain the whole pipeline.

`self.logIn()` : user detail capture, WebDriver initialisation, Instagram log in.

`self.getLinks()` : gets n unique links containing <#HASHTAG> using WebDriver.

`self.getData()` : implements multi-threaded scraping of data from self.getLinks using a combination of Selenium WebDriver and Beautiful Soup. Method returns a pandas DataFrame

# InstagramGraph()
`csv,source_col='searched_for',post_col='post',user_col='user'`
---  
This class is made up of a series of methods that take the DataFrame from InstagramScraper(). The methods below need to be called sequentially.T here is no current method to chain the whole pipeline.

- *csv*: specify path for input csv

- *source_col*: specify which column contains the hashtag that was searched for. The label.

- *post_col*: specify which column contains the Instagram post.

- *user_col*: specify which column contains the user


`self.getFeatures(translate=False)`: creates various descriptive metrics from the data.

- *translate*: if True, will access the Google Translate API and identify post language. Due to API request limits, this may take some time depending on the volume of data.


`self.selectData(english=True,remove_verified=True,max_posts=3,lemma=True)`:Subsets the data across various variables.

- *english*: only include English language posts

- *remove_verified*: removes verified Instagram users from the data set i.e brands

- *max_posts* : set a threshold for user post frequency i.e. high volume posters

- *lemma* : lemmatise hashtags where possible


`self.buildGraph(additional_stopwords=[],min_frequency=5)`: generates edges and nodes and adds them to an instance of a NetworkX graph object.

- *additional_stopwords*: There are default stopwords however extra ones that are relevant to the topic scraped can be added.

- *min_frequency*: Refers to an edge frequency threshold. The lower the min_frequency, the longer it takes to build the graph and the more potential noise will exist in the model. The higher the frequency the more 'bigger' picture the ouputs will be - albeit with less detail. Experiment with this.


`self.plotGraph(sizing=75,node_size='adjacency_frequency',layout=nx.kamada_kawai_layout,light_theme=True,colorscale='Viridis',community_plot=False)`:

- *sizing*: modify this to change the relative size of all nodes in the Plotly Scatterplot

- *node_size*: choose a graph metric to represent the plot node size - betweeness_centrality,clustering_coefficient are alternatives to the default.

- *layout*: choose the nx.layout to plot

- *light_theme*: different Plot styles

- *colorscale*: colourscale for gradient colouring

- *community_plot*: colours nodes as per community allocation if set True


`self.plotCommunity(colorscale=False)`: creates a sunburst plot of communities and contributing hashtags

- *colorscale*: implements single colour scale of 'betweeness_centrality' if True


`self.savePlot(plot='map')`: saves plots as HTML to local directory. Use 'community' if community sunburst plot needs to be saved

`self.saveTables()`: saves all csv files to local directory - node DataFrame, edge DataFrame, initial processed DataFrame and selected DataFrame
