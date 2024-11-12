# bucket query
 
 db.test.aggregate([
    // stage-1
    {
        $bucket: {
              groupBy: "$age",
              boundaries: [ 20, 40, 60, 80 ],
              default: "OtherAges",
              output: {
                "count": { $sum: 1 },
                "fullDocs" : { $push: "$$ROOT" }
              }
            }
    },
    // stage-2
    { $sort: {count: -1} },
    // st-3
    { $limit: 2 },
    // stage-4
    { $project: {count: 1, "fullDocs.name": 1} }
])