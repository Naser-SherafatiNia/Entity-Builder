# Entity-Builder

:tomato: Laravel code generator + GUI: Intuitive Convenient Efficient [Online](https://googee.github.io/Entity-Builder/dist)

:star2: Please leave a star if you find it helpful.


## Feature

- Load / Save Project JSON
- Zip and download PHP code
- Deploy PHP code online
- Customize PHP templates
- Load from database schema
- Design table fields and indexes
- Define model factories
- Manage model relations
- Set field validation rules
- Draw Diagram
- Generate Controller
- Generate Factory
- Generate Migration
- Generate Model
- Generate Policy
- Generate Request
- Generate Resource
- Generate Seeder
- Generate Test
- Generate React Form
- Generate Vue Form

![table](https://github.com/GooGee/Entity-Builder/raw/gh-pages/table.gif)


## Project setup
```
npm install
```

Compiles and hot-reloads for development
```
npm run serve
```

Compiles and minifies for production
```
npm run build
```

Lints and fixes files
```
npm run lint
```


## Deploy PHP code online

If you want to deploy PHP code online, then use this [package](https://github.com/GooGee/Entity)


## Template Document

[Document](https://mozilla.github.io/nunjucks/templating.html)


## Entity Document

[Document](https://googee.github.io/Entity-Core/docs/)

Structure of [Project](https://googee.github.io/Entity-Core/docs/classes/project.html)

- FileTypeManager
  - list: Array of FileType
    - [FileType](https://googee.github.io/Entity-Core/docs/classes/filetype.html)
      - PropertyManager
        - list: Array of Property
          - [Property](https://googee.github.io/Entity-Core/docs/classes/property.html)

- EntityManager
  - list: Array of Entity
    - [Entity](https://googee.github.io/Entity-Core/docs/classes/entity.html)
      - FieldManager
        - list: Array of Field
          - [Field](https://googee.github.io/Entity-Core/docs/classes/field.html)

      - IndexManager
        - list: Array of Index
          - [Index](https://googee.github.io/Entity-Core/docs/classes/index.html)

      - RelationManager
        - list: Array of Relation
          - [Relation](https://googee.github.io/Entity-Core/docs/classes/relation.html)

      - FileManager
        - list: Array of File
          - [File](https://googee.github.io/Entity-Core/docs/classes/file.html)
