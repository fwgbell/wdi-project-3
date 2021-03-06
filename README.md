# General Assembly WDI Project 3: FilmSpotting

[GitHub](https://github.com/fwgbell/wdi-project-3)

[Heroku Link](https://film-spotting.herokuapp.com/#!/)

At the start of the third project our instructors split the class in to small groups mixing up people with different strengths and weaknesses. Then we were tasked with creating a fully RESTful web app using an Angular.JS front-end. As a team we all had a common interest in movies so decided that would be the focus of our project and after some planning we came up with Film Spotting, a movie database / social media mashup to see what films the people you follow rate highly. Working in a group was challenging at first as we didn’t have as much freedom to code how we were used to with previous projects. However after learning to divide the work up in a way that suited us and figuring out how to code in way we all agreed with this project was really enjoyable and I’m proud of what our group was able to produce.

___


## Brief

Your instructors will partner you with other classmates to design and collaboratively build a MEAN stack app of your own design.

Your app must:

* Use Mongo, Node & Express to build a server-side API
* Your API must have at least 2 related models, one of which should be a user
* Your API should include all RESTFUL actions for at least one of those models
* Include authentication to restrict access to appropriate users
* Include automated tests for at least one resource
* Use Angular to build a front-end that consumes your API
* Use SCSS instead of CSS
* Use Webpack & Yarn to manage your dependencies and compile your source code

---

## Technologies Used:

* HTML5
* SCSS
* JavaScript(ECMAScript 6)
* Node.js
* AngularJS
* Bulma
* MongoDB
* Express
* Mongoose
* Chai
* Mocha
* Git
* Heroku
* Trello

---

## Screenshot Walk-through

### Home page

![Home page](https://i.imgur.com/znrZfl6.png)

### The film index page, used to find a film on the site

![Index page](https://i.imgur.com/M2PMxNh.jpg)

### This is the show page for a film, here you can read reviews or write one of your own

![Show page](https://i.imgur.com/ffUWUdQ.png)

### This is the news feed where you can scroll through the most recent reviews written by the people you follow

![Feed page](https://i.imgur.com/GiKTuVr.png)

___

## Approach Taken

To help keep track of who was working on what and what else needed to be done we used a Trello board for this project. We would break down each section of the project in to tasks and divide them up between us based on our perceived strengths and weaknesses.

### Our Trello board

![Trello board](https://i.imgur.com/pNVfTvx.png)

#### Featured piece of code 1

This is a piece of code I wrote for the feed controller. It sorts through the users followed by the current user and pulls all their reviews. Then it saves relevant details to the review and pushes it to the feed content array. The feed content is then displayed on the page.

``` JavaScript
$scope.feedContent = [];
$scope.following.forEach(function(user){
  user.moviesReviewed.forEach(function(movie){
    movie.reviews.forEach(function(review){
      if(review.createdBy === user._id){
        review.movieId = movie._id;
        review.movieName = movie.name;
        review.movieImage = movie.image;
        review.creatorId = user._id;
        review.creatorUsername = user.username;
        review.creatorProfilePicture = user.profilePicture;
        $scope.feedContent.push(review);
      }
    });
  });
});
```



#### Featured piece of code 2

This snippet of code is a function I wrote to handle when a user likes a review.
First it checks if a user is authenticated because a none logged in user can't like or dislike. Then the first part of the if block checks if the user has already liked the review. If they have then they are unliking it so it removes their user id from the likeBy array and then updates the review in the backend. If they haven't already liked the review then the else section of the block will push their ID in to the likedBy array and update it.

``` JavaScript
$scope.like = function(review){
  if($auth.isAuthenticated() && review.createdBy._id !== $scope.userId){
    if(review.likedBy.includes($scope.userId)){
      const index = review.likedBy.indexOf($scope.userId);
      review.likedBy.splice(index, 1);
      $http({
        method: 'PUT',
        url: `/api/movies/${$state.params.id}/reviews/${review._id}`,
        data: review
      }).then(() => $state.go('movieShow', { id: $state.params.id }));
    }else{
      review.likedBy.push($scope.userId);
      $http({
        method: 'PUT',
        url: `/api/movies/${$state.params.id}/reviews/${review._id}`,
        data: review
      }).then(() => $state.go('movieShow', { id: $state.params.id }));
    }
  }
};
```
___

## Wins and Blockers

I think our biggest win was how well we all worked together as a team. Being able to help cover each others weaknesses and take on board each others advice meant that this project really is a true collaboration of all of us.

In terms of elements that I built, the biggest win was probably the news feed. I'm pleased with how it displays the reviews of only the people that you're following and I think it's one of the features that really makes this project stand out from just another movie database.

My biggest blocker was testing. I had only done this a little bit before, so it was hard to know if the test was written incorrectly, or if the code was wrong when the tests broke. It was also hard to know exactly what to test for, particularly on the authentication side.

___
## Future Features

With more time we wanted to add mobile responsiveness to the design to make the sight more appealing and usable on mobile devices. We also planned to consume more API's namely google maps to make a cinema locator and the online movie database to pull ratings and information about the films from other sources.
