# travel_recommendation
Creation of a recommendation system for travels

The idea is to develop a system that suggests new cities and activities for someone based on their own past experiences and a database from other users' experiences. Such a dataset is not openly available to people outside companies like TripAdvisor or GetYourGuide. So we created one from scratch.


The first step was to create a large number of cities. 'City0001' and 'City0002' are boring names, so we found a list of fictional cities with 987 names. For each of them, we randomly selected (lognormal distribution) a number of activites available. We capped this number at 50 and all had at least 1, and the mode of the distribution was less than 10. We tagged each activity from a list of 10 different categories. Our end result here is a table with rows as cities and each column is one of the activites for that city, where the category of the activity is presented.


The next step was to create the users. Again, we found a list of every name that show up in the Brazilian Census, with more than 64k names. For each of them, we randomly selected (lognormal distribution) a number of cities they already visited. In each city, we randomly selected a number of activities available to add to our user history. Our end result here is a table where each row shows one user experience, with Name, City, Activity and Category.


Now we have a proper dataset to start our recommendation system. Here we have two approaches: vectorization and associative rule learning.


For the vectorization, we translated our two tables into new tables where the rows are cities (or user names) and the columns are the proportion for each category. This way we have a vector representing each city and how its activities are distributed in the categories. For example, we can have a city that has more Food attractions, while another has more Hiking activities. We get the same thing for the users, where each column shows what are the user preferences. Someone mught be more inclined to visit Historic sites than Sports.


Using Cosine Similarity, we can compare all the cities and see what cities have the same kind of attractions, and we can compare all users and see people who have the same preferences. With vectorization, we can also compare cities with users, and vice versa. We can search for cities that have activities with the same distribution of a certain user's experiences, or find users that would find a particular city interesting.


For the associative rule learning, we use the library *apyori* to compute the lift between a pair of cities. For a given city, it returns a list with descending lift, indicating what other locations are usually visited together with the original one. We also compute the lift between a pair of activities. That is, we get a list of activities that are usually visited by the same user.


With these two approaches, we are ready to develop our recommendation system. The input for our function is the name of a user. From this user, we get the list of cities they have already visited. We use our associative rule learning to find the five cities with the highest lift to any of the visited cities. If there are less that five cities, we fill the gaps with the vectorization approach. For each of these cities, we look for activities that have a high lift the activities already visited by the user.  In this case, we don't fill the gaps if we can't find five activities.


The end result for our recommendation system is a table where each column is a city, ordered from first to fifth suggestion, where each row is the activities that match the user's experience. We can have zero attractions, when we can't find any associative rule, up to five, with their categories.
