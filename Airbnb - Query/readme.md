mongoimport --db airbnb --collection listing --file airbnb.json --port 27017

Problem
use the airbnb sample data
make the following queries

1. write a query of using two OR operators, and check for listings from any two countries
==> db.air.find({$or:[{'address.country': "Brazil"},{'address.country': "Canada"}]},{'address.country': 1,_id:1}).count()
==> db.air.find({$or:[{'address.country': "Brazil"},{'address.country': "Canada"}]},{'address.country': 1,_id:1})

2. write a query of using two AND operators and check for one country and review ratings to be greater than a particular value
==> db.air.find({$and:[{'address.country': "Brazil"},{'review_scores.review_scores_rating': {$gt:95 }}]},{'address.country': 1,_id:0,'review_scores.review_scores_rating':1}).count()
==> db.air.find({$and:[{'address.country': "Brazil"},{'review_scores.review_scores_rating': {$gt:95 }}]},{'address.country': 1,_id:0,'review_scores.review_scores_rating':1})


3. write a query of using AND and OR operator in conjuction
    a. it should belong to any two cities and
    b. it should have rating = 99
==> db.air.find({$and:[{$or:[{'address.country':"Brazil"},{'address.country':"Canada"}]},{'review_scores.review_scores_rating':{$gte:99}}]},{'address.country': 1,_id:0,'review_scores.review_scores_rating':1}).count()
 298
==> db.air.find({$and:[{$or:[{'address.country':"Brazil"},{'address.country':"Canada"}]},{'review_scores.review_scores_rating':{$gte:99}}]},{'address.country': 1,_id:0,'review_scores.review_scores_rating':1})



4. write a query of using AND and OR operator together
    a. it should belong to US and rating of 95 or
    b. it should belong to CA and rating of 99
==> db.air.find({$or:[{$and:[{'address.country_code':"US"},{'review_scores.review_scores_rating':95}]},{$and:[{'address.country_code':"CA"},{'review_scores.review_scores_rating':99}]}]},{'address.country': 1,_id:0,'review_scores.review_scores_rating':1}).count()
 99
==> db.air.find({$or:[{$and:[{'address.country_code':"US"},{'review_scores.review_scores_rating':95}]},{$and:[{'address.country_code':"CA"},{'review_scores.review_scores_rating':99}]}]},{'address.country': 1,_id:0,'review_scores.review_scores_rating':1})


5. write a query of using querying arrays
    a. check all listings which have amenities of size 5, 6, 7, 8, 9, 10
    ==>  db.air.find({$or:[{'amenities':{$size:5}},{'amenities':{$size:6}},{'amenities':{$size:7}},{'amenities':{$size:8}},{'amenities':{$size:9}},{'amenities':{$size:10}}]},{_id:0,amenities:1,'address.country_code':1}).count()
    657
     db.air.find({$or:[{'amenities':{$size:5}},{'amenities':{$size:6}},{'amenities':{$size:7}},{'amenities':{$size:8}},{'amenities':{$size:9}},{'amenities':{$size:10}}]},{_id:0,amenities:1,'address.country_code':1})


    b. check all listings of where aminities contain 4 or 5 different listings
    ==> db.air.find({$or:[{'amenities':{$size:4}},{'amenities':{$size:5}}]},{_id:0,amenities:1,'address.country_code':1}).count()
    47
    ==> db.air.find({$or:[{'amenities':{$size:4}},{'amenities':{$size:5}}]},{_id:0,amenities:1,'address.country_code':1})


    c. check all listings of reviews where it matches a particular user id and name
    ==> db.air.find({$and:[{'reviews.reviewer_id':'52006105'},{'reviews.reviewer_name':"Antoine"}]}).count()
    1
    ==>db.air.find({$and:[{'reviews.reviewer_id':'52006105'},{'reviews.reviewer_name':"Antoine"}]})


    d. check all listings of reviews where it matches a particular user id and name, list only reviews that match the particular user id and name
    ==> db.air.find({$and:[{'reviews.reviewer_id':'52006105'},{'reviews.reviewer_name':"Antoine"}]},{reviews:1,_id:0}).count()
    1
    ==>db.air.find({$and:[{'reviews.reviewer_id':'52006105'},{'reviews.reviewer_name':"Antoine"}]},{reviews:1,_id:0})


6. find top 10 listings, sort them according to rating in descending order, and if the rating is similar rate them according to alphabetic order

    a. also ensure results belong only to New York, and check if there are atleast Wifi, Breakfast and 2 other amenities
    ==> db.air.find({$and:[{'address.market':"New York"},{ 'amenities':{$all:["Wifi","Breakfast","Shampoo","TV"]} } ]},{'amenities':1,'address.market':1,'review_scores.review_scores_rating':1,_id:0}).sort({'review_scores.review_scores_rating':-1, name:1}).count()
    28

    ==> db.air.find({$and:[{'address.market':"New York"},{ 'amenities':{$all:["Wifi","Breakfast","Shampoo","TV"]} } ]},{'amenities':1,'address.market':1,'review_scores.review_scores_rating':1,_id:0}).sort({'review_scores.review_scores_rating':-1, name:1})
