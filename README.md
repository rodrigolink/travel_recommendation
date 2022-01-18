# travel_recommendation
Creation of a recommendation system for travels

The idea is to develop a system that suggests new cities and activities for someone based on their own past experiences and a database from other users' experiences. Such a dataset is not openly available to people outside companies like TripAdvisor or GetYourGuide. So we created one from scratch.


The first step was to create a large number of cities. 'City0001' and 'City0002' are boring names, so we found a list of fictional cities with 987 names. For each of them, we randomly selected (lognormal distribution) a number of activites available. We capped this number at 50 and all had at least 1. 
