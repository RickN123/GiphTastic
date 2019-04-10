
<!--------->

<head>
    <meta charset="utf-8">
    <title>Favorite Teams</title>
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css"
        integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
</head>

<body>
    <div class="width=100% p-3 mb-2 bg-primary text-white text-center height">
        <h1>Favorite Philadelphia Teams</h1>

        <img class="images-returned" team-still="https://media3.giphy.com/media/"
            team-animate="https://media3.giphy.com/media/mO9vPApYnQa1a/200.gif?cid=e1bb72ff5cad4168313346672ec2b3d5"
            team-state="still" class="gif">

        <div id="buttons-view">

        </div>
        <!----settting up the form for the team input-->
        <form id="team-form">
            <label for="team-input">Enter Your Favorite Sports Teams!</label>
            <input type="text" id="team-input"><br>

            <input id="add-team" type="submit" value="Add Your Favorite Team!">

        </form>
        <!---First attempt trying to do just buttons-->
        <div id="buttons">
            <!--<button data-team="Philadelphia Flyers">Get Flyered Up!</button>
            <button data-team="Rocky">Aaaadrian!</button>
            <button data-team="76ers">10-9-8-76ers!</button>
            <button data-team="Philadelphia Eagles">Fly Eagles Fly!</button>
            <button data-team="Villanova Wildcats">Go Cats!</button>-->
        </div>
        <!---- populating the gifs to display-->
        <div id="gifs-appear-here">
        </div>

        <br>

        <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
        <script type="text/javascript">
            var teams = ["Philadelphia Flyers", "Rocky", "76ers", "Philadelphia Eagles", "Villanova Wildcats"];
            /// setting up additional team on click event and function ---this was a bit more difficult then going off just having buttons set
            $("#add-team").on("click", function (event) {
                /// avoid submitting the form 
                event.preventDefault();
                ///keeps the submission to the value and removing the spaces from beginning and end
                var team = $("#team-input").val().trim();
                ///pushing the submission to the array
                teams.push(team);
                /// driving the button creation
                renderButtons(team);
            });

            function renderButtons() {
                $("#buttons").empty();
                ///looping through the teams array until it hits the end and then appends a button
                for (var i = 0; i < teams.length; i++) {
                    var team = teams[i]
                    $("#buttons").append("<button>" + team + "</button>")
                }
            }/// first render of buttons occurs
            renderButtons();
            ///populates the gifs
            function renderGifs(team) {
                var queryURL = "https://api.giphy.com/v1/gifs/search?q=" + team + "&api_key=dc6zaTOxFJmzC&limit=10";
                ///starts the query url to go to the library
                $.ajax({
                    url: queryURL,
                    method: "GET"

                }).then(function (response) {
                    $("#gifs-appear-here").empty()
                    ///populating the rating through a loop
                    var results = response.data;

                    for (var i = 0; i < results.length; i++) {

                        if (results[i].rating !== "r" && results[i].rating !== "pg-13") {

                            var gifDiv = $("<div>");

                            var rating = results[i].rating;

                            var p = $("<p>").text("Rating: " + rating);
                            ///replacing with static image - appending for static gif due to dynamic URL---this was a nightmare compared to the pausing gif due to the dynamic nature
                            var teamImage = $("<img>");
                            teamImage.attr('team-animate', results[i].images.fixed_height.url);
                            teamImage.attr('team-still', results[i].images.fixed_height.url.replace('.gif', '_s.gif'));
                            teamImage.addClass("gif")

                            teamImage.attr("src", results[i].images.fixed_height.url);
                            ///bryan - create another attribute called data-still will have url of still image
                            ///create another attribute called data-animate will have url of image
                            ///create another attribute called data-state will be set to still or animate


                            gifDiv.append(p);
                            gifDiv.append(teamImage);
                            ///loads the image in the static form before click - pulled into the function
                            $("#gifs-appear-here").prepend(gifDiv);
                            teamImage.attr("src", teamImage.attr("team-animate"));
                            teamImage.attr("data-state", "animate");

                        }
                    }
                });
            }



            $(document).on("click", "button", function () {
                // var team = $(this).attr("data-team");
                var team = $(this).html();
                renderGifs(team);
            });


            // Gif click
            $(document).on("click", ".gif", function () {
                // bryan - The attr jQuery method allows us to get or set the value of any attribute on our HTML element
                var state = $(this).attr("data-state");
                // If the clicked image's state is still, update its src attribute to what its data-animate value is.
                // Then, set the image's data-state to animate
                // Else set src to the data-still value
                if (state === "still") {
                    $(this).attr("src", $(this).attr("team-animate"));
                    $(this).attr("data-state", "animate");
                } else {
                    $(this).attr("src", $(this).attr("team-still"));
                    $(this).attr("data-state", "still");
                }
            });
        </script>
</body>

</html># GiphTastic