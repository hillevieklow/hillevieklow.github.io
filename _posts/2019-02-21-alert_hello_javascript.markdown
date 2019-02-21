---
layout: post
title:      "alert( 'Hello, JavaScript!' );"
date:       2019-02-21 14:19:46 -0500
permalink:  alert_hello_javascript
---

For my fourth and second to last portfolio project here at Flatiron School, I was instructed to add dynamic features to my previous project [Bartender App](http://https://github.com/hillevieklow/bartender-rails-app). These features are made possible through JavaScript with JQuery library, and of course JSON API. 

The whole idea behind my Ruby on Rails project was to create an App that would allow users to share cocktail recipes, and find new ones either by browsing the Recipes page or through filter by a certain ingredient. 

With the adding of Javascript, I wanted to add the features of writing reviews on the recipes, have a "read more" link that renders more information without refresh, and the ability to move between different recipes (without reload!) with "next" and "previous" buttons.

Here are the specific project requirements, a description of how I implemented them in my app, and a few examples.

--------------------------------------

**Must have a Rails Backend and new requirements implemented through JavaScript.**
Well.. yes :) 

**Makes use of ES6 features as much as possible(e.g Arrow functions, Let & Const, Constructor Functions)**
Essentially, this requirement is met just by using JavaScript the way I was taught here at Flatiron School. Note that it is important to keep in mind all of ES6's quirks whenever googling for answers - *there is a great chance that the tutorial you're watching outside Learn.co aren't using ES6 features.*

**Must translate the JSON responses into Javascript Model Objects using either ES6 class or constructor syntax.**
I did this twice in my application, once for the RecipeIngredient model, and once for the Review model. This is how I implemented it in code for Review: 
```
// app/assets/javascripts/application.js
// ES6 Review class (model object) initialized using constructor method

  class Review{
    constructor(data){
    this.id = data.id;
    this.title = data.title;
    this.content = data.content;
    this.user = data.user;
    }
  }
```

**Must render at least one index page (index resource - 'list of things') via JavaScript and an Active Model Serialization JSON Backend.**
I decided to deal with this requirement through rendering the index page of Recipes. To do this, I hade to use the 'active_model_serializers' gem and create a serialization of my Recipe model. I tweaked my recipe-index page to look like this: 

```
// app/views/recipes/index.html.erb

<div class="row outlined-section" id="recipeIndex">
  <h1>All Recipes (<%= Recipe.all.count%>)</h1>
  <div id="recipesInfo">
  </div>
</div>
```

Together with the following Javascript, each recipe is presented, and if the user clicks on 'read more', the entire recipe description is shown: 

```
// app/assets/javascripts/application.js

  if ($("#recipesInfo").length) {
    loadAllRecipes();
  }

  // Loads all the recipes with descriptions
  function loadAllRecipes(){
    $.ajax({
      url: "/recipes.json",
      method: 'GET'
    })
    .then(function(data){
       let recipeArray = data;
       $.each(recipeArray, function(index, recipe){
           let recipeData = `<h4><a href='/recipes/${recipe.id}'> ${recipe.name}</a>,
                            ${recipe.upvotes} upvotes</h4><div id='description-${recipe.id}'>
                            <p>${recipe.description.substring(0, 120)}...
                            <a href='#' data-id=${recipe.id} class='js-more'>Read More</a>
                            </p></div><br>`
           $('#recipesInfo').append(recipeData);
         })
     });
  }

  // Displays the whole description when "read more" is clicked
  $("#recipesInfo").on('click', '.js-more', function(e){
    e.preventDefault();
    let id = this.dataset.id;
    $.get(`/recipes/${id}.json`, function(data){
      $(`#description-${id}`).html(data.description)
    });
  });
```

**Must render at least one show page (show resource - 'one specific thing') via JavaScript and an Active Model Serialization JSON Backend.**
When on the recipe-show page, a user can click two buttons; next and previous, and another recipes show page is rendered via JS and an Active Model Serialization JSON Backend. 

**Your Rails application must reveal at least one has-many relationship through JSON that is then rendered to the page.**
In my app, a recipe has many reviews, and a recipe has many recipe-ingredients.

**Must use your Rails application to render a form for creating a resource that is submitted dynamically through JavaScript.**
As discussed above, I decided to add a review feature.  This is what the JavaScript to achieve this looks like:

```
  $(function() {
    $("form#new_review").on("submit", function(e){
      e.preventDefault();
      let $form = $(this);
      let action = $form.attr("action");
      let params = $form.serialize();
      $.ajax({
        url: action,
        data: params,
        dataType: "json",
        method: "POST"
      })
      .success(function(json){
        $(".reviewBox").val("");
        let newReview = new Review(json);
        let reviewHtml = newReview.renderDisplay();
        $("#submitted-reviews").append(reviewHtml);
      })
    });
  })

});
```

**At least one of the JS Model Objects must have a method on the prototype.**
The method I decided to add on the Review prototype is a renderDisplay method, all it does is returning a string using the *this* keyword to access the instance's values. In other words, when this function is called, it returns HTML than can be appended to the DOM using JQuery (see JS above). 

```
// app/assets/javascripts/application.js

  Review.prototype.renderDisplay = function(){
    return (`<div class="well well-white" id="review-${this.id}"><h4>${this.title}</h4><p>${this.content}</p><strong>- ${this.user.name}</strong></div>`)
  }
```

--------------------------------------

Overall, I am happy with the new functionality of my Bartender App, and I had a great time adding it and playing around with mixing Ruby on Rails with JavaScript.

Until next time,
Hillevi 



