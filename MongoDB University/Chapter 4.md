# ------------------- #

Lab 1.
accommodates
number_of_reviews

db.listingsAndReviews.find(
    {"$and": [
        {"accommodates": {"$gt": 6}},
        {"number_of_reviews": 50}
    ]}
)

db.listingsAndReviews.aggregate(
    {"$match": {
        "$and": [
        {"accommodates": {"$gt": 6}},
        {"number_of_reviews": 50}]}
    },
    {"$project": {"_id": 0, "name": 1, "accommodates": 1, "number_of_reviews": 1}}
)

# ------------------- #

Lab 2.
property_type = House
amenities = Changing table

db.listingsAndReviews.find(
    {"$and": [
        {"property_type": "House"},
        {"amenities": "Changing table"}
    ]}
).count()

# ------------------- #

Quiz: Array Operators
bedrooms

db.listingsAndReviews.find(
    {"$and": [
        {"bedrooms": {"$gte": 2}},
        {"amenities": {"$all": ["Free parking on premises", "Air conditioning", "Wifi"]}}
    ]}
).count()

# ------------------- #

Lab: Array Operators and Projection
offices.city

db.companies.find(
    {"offices": {"$elemMatch": {"city": "Seattle"}}}
).count()

db.companies.find(
    {"offices": {"$elemMatch": {"city": "Seattle"}}},
    {"offices.city": 1}
)

# ------------------- #

Quiz: Array Operators and Projection

db.companies.find(
    {"funding_rounds": {"$size": 8}},
    {"_id": 0, "name": 1}
)

# ------------------- #

Lab 1: Querying Arrays and Sub-Documents
start station location.coordinates

db.trips.find({ "start station location.coordinates.0": { "$lt": -74 }}).count()

# ------------------- #

Lab 2: Querying Arrays and Sub-Documents

db.inspections.find(
    {"address.city": "NEW YORK"}
).count()