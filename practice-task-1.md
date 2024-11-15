### task-1

db.test.find({ age: { $gt: 30 } })
    .project({ name: 1, email: 1, age: 1 })

    