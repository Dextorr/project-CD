# General Assembly Project 2 : React Hackathon

Charlotte Morgan | Dexter De Leon

### Project Brief
A pair-coded project to develop a React app using external APIs.

### Timeframe
2 days

## Technologies used

* React
* JavaScript (ES6)
* Webpack
* Bulma
* HTML 5
* SASS/SCSS/CSS Animation
* Git/GitHub

## Deployed version
- http://bit.ly/DDLProjCD

## Code Installation

- http://bit.ly/DexP2


1. Clone or download the repo
2. Install ```yarn``` in Terminal
3. Start Webpack Dev Server with ```yarn run serve```

## Overview
A music search/recommendation engine built in React and using third-party APIs. Developed with a partner in a hackathon challenge at General Assembly on the Web Development Immersive course. We were given two days to make something using React, using any publicly available APIs we could get access to. This project makes use of the Musixmatch and Last.fm APIs to search for artists, return their top albums/track listings and recommend a number of similar artists.

## User Journey
The app begins on this page, with a single search bar which allows you to enter an artist name.

![Screenshot Project CD Landing](https://user-images.githubusercontent.com/44480965/63770693-a2ff4500-c8cd-11e9-804f-8f1a98b9b423.png)

Upon typing an artist name and clicking on the "Submit" button, you will be given the top 5 search results.

![Screenshot 2019-08-27 at 13 26 24](https://user-images.githubusercontent.com/44480965/63771038-4bada480-c8ce-11e9-932f-394e9371f502.png)

Clicking on any of the search results will bring you to the artist page, which displays their bio and top albums.

![Screenshot 2019-08-27 at 13 27 58](https://user-images.githubusercontent.com/44480965/63771172-87e10500-c8ce-11e9-994b-0feb3ee70132.png)

Scrolling to the bottom of the artist page, you will see a number of similar artists results. Clicking on any of these results will bring you to the relevant artist's page.

![Screenshot 2019-08-27 at 13 34 02](https://user-images.githubusercontent.com/44480965/63771582-59aff500-c8cf-11e9-907b-4228e9433d51.png)

Clicking on the "Search again" button will return you to the search page, and clicking on any of the albums will bring up that album's track listing (if it is available on Last.fm).

![Screenshot 2019-08-27 at 13 30 54](https://user-images.githubusercontent.com/44480965/63771347-ed34f600-c8ce-11e9-9048-75412775c672.png)

Clicking on the "Listen" button for any of these tracks will open that track's page on Last.fm in a new tab.

# Process

Our first task was to find an API that served data that we could work with, so we searched for APIs that had data we were interested in. Charlotte found some APIs related to live events and also checked out what NASA had to offer, while I looked at things like quotes APIs and image searching.

Eventually we settled on doing something music related and we both started looking for APIs which would serve that purpose.

We discovered the Last.fm and Musixmatch APIs and began using Insomnia to find out what the data responses looked like.

![Screenshot 2019-08-27 at 13 41 00](https://user-images.githubusercontent.com/44480965/63772099-5406df00-c8d0-11e9-81ba-455c176a1217.png)

![Screenshot 2019-08-27 at 13 41 50](https://user-images.githubusercontent.com/44480965/63772169-73057100-c8d0-11e9-81e3-3d28976842fe.png)

After looking through the data a bit, we found that the Last.fm API offered much more complete and interesting data, but Musixmatch had a much more reliable search feature, so we decided to use both APIs for our project.

Searching from the main page will run the search string through the following code:

```javascript
handleSubmit(e){
  e.preventDefault()
  if(e.target.value !== ''){
    axios.get(`https://api.musixmatch.com/ws/1.1/artist.search?apikey=${musixKey}&q_artist=${this.state.search}`)
      .then(res => this.setState({ artists: res.data.message.body.artist_list.slice(0, 5) }))
      .catch(error => alert(error))
    this.setState({ submit: true })
  }
}
```

Passing the search term to the Musixmatch search endpoint, our app then sets state with the five top search results from the response for that search term and will render the results in a `SearchResults` component.

Clicking on one of the search results will add that artist's name to the URL through a `react-router-dom` `Link`, which allows it to be used by the `ArtistShow` component:

```javascript
// ArtistShow Component constructor
constructor(props){
  super(props)

  this.state = {
    albums: [],
    artistInfo: {}
  }
  this.title = this.props.match.params.artist
}
```

The `this.title` variable, taken from the URL (Musixmatch search result), in this component is used to get the artist's data from the Last.fm API, which is rendered in the component, showing the artist bio, top albums and similar artists.

From this point all data is taken from the Last.fm API which has all the artist's albums, track listings, images and similar artist recommendations.

On the `ArtistShow` component, we wanted to re-use the `SearchResults` component to display the similar artists from the Last.fm API, passing it the similar artists instead of search results, but ran into issues with the 'react-router-dom' routes, because the route still had the same `/path/:artist` format and therefore wouldn't re-render the component.

To get around this we had to instead render the same element as the `SearchResults` component, but with an additional click event that would get the new artist data from the Last.fm API by running the component's `getData` function. This would cause a change in state and would then re-render the component with the new information.

```javascript
//Inside the component's render function
<ul id="searchResultList">
  {this.state.artistInfo.artist.similar.artist.map((artist, i) =>
    <Link key={i} to={`/${artist.name}`} onClick={this.getData}>
      <li id={`a${i}`}>{artist.name}</li>
    </Link>
  )}
</ul>
```

### Challenges

At first we had a lot of trouble working with the data from these APIs. Most of all, the Last.fm API could be difficult in returning data from artist names, despite apparently having an 'autocorrect' feature. This is the main reason for our use of an additional API to handle the artist search feature, as the Last.fm endpoints worked much more reliably with artist names in a particular format.
We were able to achieve a more uniform format by passing the user's search term string to the Musixmatch API first and then using the response to inform our requests to the Last.fm API.

### Wins

Once we had gotten over the hurdle of our data issues, it was a lot of fun to work on the look and feel of the app and how the data was to be presented.

In particular, the Last.fm API had a lot of links to images, which helped to greatly improve the look of our app.

## Future Features

We had considered to use the spotify API to be able to play tracks in-app rather than having to re-direct to another site, but were unable to attain a key in time for the deadline, it would be nice to add this functionality to the app and maybe even the option to search by track or album titles as well as artist name.
