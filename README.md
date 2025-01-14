<p align="center"><img src="https://raw.githubusercontent.com/MagicalStrangeQuark/MagicalStrangeQuark/master/assets/mongo.svg" width="400"></p>

<p align="center">Repository containing the queries developed in <a href="https://www.mongodb.com">👉 MongoDB 👈</a></p>

<p align="center">
    <a href="https://opensource.org/licenses/MIT">
        <img alt="License" src="https://img.shields.io/badge/License-MIT-yellow.svg">
    </a>
    <a href="#">
        <img alt="License" src="https://img.shields.io/github/languages/count/MagicalStrangeQuark/MongoDB">
    </a>
    <a href="#">
        <img alt="License" src="https://img.shields.io/github/last-commit/MagicalStrangeQuark/MongoDB">
    </a>
    <a href="#">
        <img alt="License" src="https://img.shields.io/github/followers/MagicalStrangeQuark?style=social">
    </a>
</p>

<h2 align="center">Website</h2>

<h3 align="center">
    <a href="https://www.mongodb.com">https://www.mongodb.com</a>
</h3>

<h2 align="center">Installation on <img src="https://raw.githubusercontent.com/MagicalStrangeQuark/MagicalStrangeQuark/master/assets/manjaro.svg" width=50 height=50 alt="Manjaro Linux"></h2>

```bash
    git clone https://aur.archlinux.org/mongodb-bin.git
```

```bash
    cd mongodb-bin
```

```bash
    makepkg -si
```

```bash
    systemctl start mongodb
```

```bash
    systemctl enable mongodb
```

<h2 align="center">Installation on <img src="https://raw.githubusercontent.com/MagicalStrangeQuark/MagicalStrangeQuark/master/assets/macos.svg" width=50 height=50 alt="MacOS"></h2>

```bash
sudo pecl install mongodb
```

```bash
brew services start mongodb-community
```

```bash
brew tap mongodb/brew
```

```bash
brew info mongodb-community
```

```bash
mongosh
```

<h2 align="center">Listing Commands</h2>

```mongodb
    help
```

<h2 align="center">Create Database / Collection [states, persons]</h2>

```mongodb
    use <database>
```

```mongodb
    use db
```

```mongodb
    db.createCollection('states')
```

```mongodb
    db.createCollection('persons')
```

```mongodb
    show dbs
```

```mongodb
    show collections
```

<h2 align="center">Droping Databases / Collection [states, persons]</h2>

```mongodb
    db.states.drop()
```

```mongodb
    db.persons.drop()
```

<h2 align="center">Inserting Data</h2>

```mongodb
    db.persons.insert({ name: "John Doe", country: "United States of America" })

    db.persons.insert({ name: "Bia", country: "Brazil" })

    db.persons.insert({ name: "Bruna", country: "Brazil", age: 24 })

    db.persons.insert({ name: "Hannah", country: "UK" })

    db.persons.insert({ name: "Lucia", country: "Brazil" })

    db.persons.insert({ name: "Maria", country: "Brazil" })

    db.persons.insert({ name: "Roberta", country: "Brazil" })

    db.states.insert({
        name: "Rio Grande do Sul",
        cities: [
            {
                _id: ObjectId(),
                name: "Novo Hamburgo",
                population: 1000
            },
            {
                _id: ObjectId(),
                name: "Porto Alegre",
                population: 5000
            },
            {
                _id: ObjectId(),
                name: "São Leopoldo",
                population: 2000
            }
        ]
    })

    db.states.insert({
        name: "Santa Catarina",
        cities: [
            {
                _id: ObjectId(),
                name: "Florianópolis",
                population: 1000
            },
            {
                _id: ObjectId(),
                name: "Joinville",
                population: 4000
            },
            {
                _id: ObjectId(),
                name: "Blumenau",
                population: 5000
            },
            {
                _id: ObjectId(),
                name: "Balneário Camboriú",
                population: 7000
            }
        ]
    })
```

<h2 align="center">Listing Data</h2>

```mongodb
    db.persons.find()

    db.persons.find().pretty()
```

<h2 align="center">Querys</h2>

```mongodb
    db.states.findOne({ name: "Rio Grande do Sul" })

    db.states.find({ name: "Rio Grande do Sul" }).pretty()
```

```mongodb
    db.persons.find({ $or: [{ name: "Bia" }, { name: "Hannah" }] }).pretty()
```

```mongodb
    db.persons.find({ $and: [{ country: "Brazil" }, { name: "Bruna" }] }).pretty()
```

```mongodb
    db.persons.find({ age: { $exists: true } })
```

<h4 align="center">Skip / Limit</h4>

```mongodb
    db.persons.find().skip(0).limit(1)
```

<h4 align="center">Count</h4>

```mongodb
    db.persons.countDocuments()
```

```mongodb
    db.persons.estimatedDocumentCount()
```

<h4 align="center">Pretty</h4>

```mongodb
    db.persons.find({ country: "Brazil" }, { name: 1, country: 1, _id: 0 }).pretty()
```

```mongodb
    db.states.find({ name: "Rio Grande do Sul"  }, { "cities.name": 1, "_id": 0 }).pretty()
```

<h4 align="center">Aggregate</h4>

```mongodb
    db.states.aggregate([
        {   $project: { name: 1, "cities.name": 1, _id: 0  }  }
    ])
```

```mongodb
    db.states.aggregate([
        { $project: { population: { $sum: "$cities.population" }, name: 1, _id: 0 } },
        { $group: { _id: null, total: { $sum: "$population" } } },
        { $project : { _id: 0, total: 1 } }
    ])
```

```mongodb
    db.states.aggregate([
        { $match: { "cities.name": "Novo Hamburgo" } },
        { $unwind: "$cities" },
        { $match: { "cities.name": "Novo Hamburgo" } },
        { $project: { _id: "$cities._id" } }
    ]).pretty()
```

<h4 align="center">Update</h4>

```mongodb
    db.persons.update( {  name: "Hannah" }, { $set: { age: 27 } } )
```

```mongodb
    db.states.update( { name: "Rio Grande do Sul" }, { $push: { cities: { _id: ObjectId(), name: "Campo bom", population: 3000 } } } )
```

<h4 align="center">Remove</h4>

```mongodb
    db.persons.remove( { name: "John Doe" }, 1)
```

```mongodb
    db.persons.remove( { age: { $exists: false } }, 1 )
```

```mongodb
    db.persons.remove( { age: { $lt: 30 } } )
```

<h4 align="center">Lookup</h4>

```mongodb
    db.createCollection('companies')
```

```mongodb
    db.companies.insert( { name: "Santander", city_id: ObjectId("5f87961d8ddbbac1f99f9783") } )

    db.companies.insert( { name: "Bradesco", state_id: ObjectId("5f87961d8ddbbac1f99f9786") } )
```

```mongodb
    db.companies.aggregate([
        { $match: { name: "Bradesco" } },
        { $lookup: { from: "states", localField: "state_id", foreignField: "_id", as: "state"   } }
    ]).pretty()
```

```mongodb
    db.companies.aggregate([
        { $match: { name: "Santander" } },
        { $lookup: { from: "states", localField: "city_id", foreignField: "cities._id", as: "state" } },
        { $unwind: "$state" },
        { $unwind: "$state.cities" },
        { $addFields: { sameCity: { $cmp: [ "$state.cities._id", "$city_id" ] } } },
        { $match: { sameCity: 0 } }
    ]).pretty()
```
