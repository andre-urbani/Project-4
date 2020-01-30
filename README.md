# artist_Flow

## Introduction

Artist flow is a fully-functional CRUD application which allows users to search for their favourite music artists, and discover new artists based on recommendations from Deezer’s API. Users are able to register their own profile, and save artists to their personal profile page, which then displays all upcoming events by each artist, sourced from the Skiddle event API. 

The application was developed using Django for the back-end and React for the front-end. This was a group project (three group members), with my main role being the development of the main “node” page which allows users to discover new artists. 


The website has been deployed via heroku, and you can try it out yourself [here](https://artist-flow-au.herokuapp.com/)


## Brief

- Build a full-stack application by making your own backend and your own front-end
- Use a Python Django API using Django REST Framework to serve your data from a Postgres database
- Consume your API with a separate front-end built with React
- Be a complete product which most likely means multiple relationships and CRUD functionality for at least a couple of models
- Implement thoughtful user stories/wireframes that are significant enough to help you know which features are core MVP and which you can cut
- Be deployed online so it's publicly accessible

## Technologies used

- React.js
- JavaScript (ES6)
- Python
- Django
- Postgres
- HTML
- CSS
- Axios
- Deezer API
- Skiddle API
- React Toastify
- Moment
- Git and GitHub
- Bulma
- Google Fonts

## Timeframe

- 7 days


## Process

### Planning

Once we had decided on the concept of the application, we began the initial planning process, first establishing the features we wanted on the platform, and then deciding on how we would integrate our own back-end functionality. 

As a group, we coded the Django backend functionality, which took approximately 2-3 days, before splitting and working on separate front end components. We continuously helped each other and cross checked our work. Individually I worked on the main "node" page, and the registration page, as well as various styling elements.

### Features

- Registration and login page 
- Home page, where users search for their favourite artists
- "Node" page, where users are presented with recommended artists based on their search
- A main node, which is the current artist, and four recommended artist nodes 
- The main node displays 3 of the artists top tracks, which the user can listen to 30 second previews of
- A 'like' button which adds the current artist to the users profile
- The ability for the user to go back one step to the previus artist they clicked on
- Selected artist gigs are displayed in the user profile page
- User can delete artists from their profile

### Walkthrough

Upon accesing the website, the user is displayed with a search bar, where they can search for any of the hundreds of thousands of artists stored in the Deezer API database. An auto-complete function is provided within the search bar. All previous searches made will also appear in the 'Recent Searches' section at the bottom of the page. These are interactive.

![search](https://i.imgur.com/2Bjepo6.png)


A 'register' and 'login' button is displayed in the top-right of the page. Clicking on 'register' will take the user to the regsitration page, where they are able to register their profile. Once this has been completed they are then sent to the login page. It is possible for the user to use the website without registering, but they will not be able to save any artists they discover to their personal profile.

![registration](https://i.imgur.com/whKb8j3.png)

Once a search has been made, the user is then directed to the 'node' page, where the artist they have searched on will be displayed in the main node. Every time the user clicks on one of the secondary artist nodes to the right, this artist will then be displayed in the main node and a fresh selection of recommendations will be provided. There are a total of 8 artist recommendations, with the user being able to click on one of the up or down arrows, above and below the secondary nodes, to access the additional recommendations.

![artist_Flow](https://i.imgur.com/1Lm335L.png)

If the user is registered and logged in, they are able to 'like' an artist by clicking on the heart button below the main node, which will then add that artist to their profile. By clicking on the 'profile' button at the top-right of the page, the user will be directed to their profile page. This page displays all of the artists the user has liked, and selecting any of these artists will display their upcoming gigs, by city, in the right-hand section of the page. Users can delete any artists they no longer require to be saved to their profile.

![profile](https://i.imgur.com/Guc4wln.png)

### Featured code

The code below highlights the chain of API calls on page load, required to populate the relevant information within each node.

```
useEffect(() => {
    thirdNode.array.push(props.location.artist.deezerId)
    axios.get(`https://cors-anywhere.herokuapp.com/api.deezer.com/artist/${props.location.artist.deezerId}`)
      .then(res => {
        const main = res.data
        setMainNode(main)
        axios.get(`https://cors-anywhere.herokuapp.com/api.deezer.com/artist/${props.location.artist.deezerId}/related`)
          .then(res => {
            const test = res.data.data.slice(0, 4)
            setSecondaryNodes(test)
            axios.get(`https://cors-anywhere.herokuapp.com/api.deezer.com/artist/${props.location.artist.deezerId}/top`)
              .then(res => {
                const tracks = res.data.data.slice(0, 3)
                setTopTracks(tracks)
              })
              .catch(err => console.log(err))
          })
          .catch(err => console.log(err))
      })
      .catch(err => console.log(err))
  }, [])
```
Additional API calls are made when the user clicks on any of the secondary nodes. This was acheived by adding an onClick event handler to each of these nodes.

```
const handleClick = useCallback((e) => {

    e.preventDefault()
    const target = e.target.getAttribute('id')
    axios.get(`https://cors-anywhere.herokuapp.com/api.deezer.com/artist/${target}`)
      .then(res => {
        const main = res.data
        setMainNode(main)
        axios.get(`https://cors-anywhere.herokuapp.com/api.deezer.com/artist/${target}/related`)
          .then(res => {
            const newSimilar = res.data.data.slice(1, 5)
            setSecondaryNodes(newSimilar)
            thirdNode.array.push(target)
            axios.get(`https://cors-anywhere.herokuapp.com/api.deezer.com/artist/${target}/top`)
              .then(res => {
                const newtracks = res.data.data.slice(0, 3)
                setTopTracks(newtracks)
                axios.get(`https://cors-anywhere.herokuapp.com/api.deezer.com/artist/${thirdNode.array[thirdNode.array.length - 2]}`)
                  .then(res => {
                    const pastArtist = res.data
                    setThirdNodeData(pastArtist)
                  })
                  .catch(err => console.log(err))
              })
              .catch(err => console.log(err))
          })
          .catch(err => console.log(err))
      })
  }, [])
```

## Result

Overall I am extremely happy with the end product, and proud of my personal contribution towards the project. I believe that the work I put into this project highlights my progression throughout my time on the software engineering course. 

I enjoyed working in a group, as you can work together to overcome any issues you may encounter. It also meant we could aim for a slightly more ambitious project in the time we had, which pushed my skills further. It was challenging working in a new language but I feel I became more comfortable with Python and Django over the duration of the project.

## Wins & Challenges

### Wins

- Being able to create the 'node' page functionality exactly as we had envisioned it at the planning stage
- Building the back end using Python and Django, a language and framework we had only been introduced to a week prior to the beginning of the project. This encouraged us to learn as we built
- Improving on my knowledge of React

### Challenges

- Coming up with a solution to allow the user to go back to the previous artist they clicked on. Once I had figured this out the coding process was fairly straightforward
- The relatively new syntax used in Python and therefore adapting myself to this language
- There were some challenging aspects of the CSS design, as it was difficult to implement exactly what I wanted in terms of animations of the nodes
