# Genearl query of the mongoDB

### $match, $project aggregations 

* aggregation syntax:
db.test.aggregate( [ ])

* geender not equal to male and age less than 30 with project method : 

db.test.aggregate(
    [
        // stage-1
        { $match: { gender: { $ne: "Male" }, age: { $lt: 30 } } },

        // stage-2
        { $project: { gender: 1, name: 1, age: 1 } }
    ])

### $addFields : 

* add field as extra fields

db.test.aggregate(
    [
        // stage-1
        { $match: { gender: "Male" } },

        // stage-2
        {
            $addFields: { course: "level -2 " }

        },

        // stage-3
        { $project: { name: 1, gender: 1, course: 1 } }
    ])

    .....................................

    db.test.aggregate(
    [
        // stage-1
        { $match: { gender: "Male" } },

        // stage-2
        {
            $addFields: { course: "level -2", eduTech: "Programming Hero" }

        },

        // stage-3
        { $project: { "name.firstName": 1, gender: 1, course: 1, eduTech: 1 } }
    ])


* create a new collection by using $out with the $addFields

db.test.aggregate(
    [
        // stage-1
        // { $match: { gender: "Male" } },

        // stage-2
        {
            $addFields: { course: "level -2", eduTech: "Programming Hero" }

        },

        // stage-3
        // { $project: { "name.firstName": 1, gender: 1, course: 1, eduTech: 1 } }
    
       // stage-4
       {$out: "course-details"}
    ])


* $merge $addFields with main collection 

db.test.aggregate(
    [
        // stage-1
        // { $match: { gender: "Male" } },

        // stage-2
        {
            $addFields: { course: "level -2", eduTech: "Programming Hero" }

        },

        // stage-3
        // { $project: { "name.firstName": 1, gender: 1, course: 1, eduTech: 1 } }

        // stage-4
        { $merge: "test" }
    ])
