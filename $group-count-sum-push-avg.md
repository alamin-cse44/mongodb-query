# $group, $count, $sum and others query 

### $group 

* $group by gender and specific gender count in total 

db.test.aggregate([
    // stage-1
    {
        $group: {
            _id: "$gender",
            totalCount: { $count: {} }
        }

    }
])

................................................

db.test.aggregate([
    // stage-1
    {
        $group: {
            _id: "$gender",
            totalCount: { $sum: 1 }
        }

    }
])

* $group by country, people names who live in the country, repeated count and the total earning of the persons in this country

db.test.aggregate([
    // stage-1
    {
        $group: {
            _id: "$address.country",
            personOfThisCountry: { $push: "$name" },
            countCountry: { $count: {} },
            totalEarnings: { $sum: "$salary" }
        }
    }
])

* $push: "$$ROOT" To see the full details of the people who live in this country

db.test.aggregate([
    // stage-1
    {
        $group: {
            _id: "$address.country",
            personOfThisCountry: { $push: "$$ROOT" },
            countCountry: { $count: {} },
            totalEarnings: { $sum: "$salary" }
        }
    }
])

* specific field display with project $project method 

db.test.aggregate([
    // stage-1
    {
        $group: {
            _id: "$address.country",
            countCountry: { $count: {} },
            totalEarnings: { $sum: "$salary" },
            fullDocs: { $push: "$$ROOT" },
        }
    },
    // stage-2
    { $project: { 
        countCountry: 1,
        totalEarnings: 1,
        "fullDocs.name": 1,
        "fullDocs.email": 1,
        "fullDocs.phone": 1,
        
    } }
])

* group with $max, $min, $avg, and $subtract

db.test.aggregate([
    // stage-1
    {
        $group: {
            _id: null,
            totalSalary: { $sum: "$salary" },
            maxSalary: { $max: "$salary" },
            minSalary: { $min: "$salary" },
            avgSalary: { $avg: "$salary" },
        }

    },
    // stage-2
    {
        $project: {
            totalSalary: 1,
            maxSalary: 1,
            minSalary: 1,
            averageSalary: "$avgSalary",
            diffMaxAndMinSalary: { $subtract: ["$maxSalary", "$minSalary"] }

        }
    }
])