# vue-quiz

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

###Fetching data from an API
```$xslt
fetch('https://opentdb.com/api.php?amount=10&category=9&type=multiple', {
   method: 'get'
}).then((response) => {
    return response.json()
}).then((data) => {
    this.questions = data.results
})
```
