# ef2go-plugin-boilerplate

Met behulp van "Vue CLI" is eenvoudig een development omgeving op te zetten zodat men meteen kan beginnen om een plugin te bouwen.

SSL staat standaard aan (selfsigned certificate). Dit omdat http resources op https omgevingen standaard door de browser geblokkeert worden. (Mixed Content).



## Plugin instellen

In het EF2GO dashboard:
Maak een nieuwe plugin een vul de benodigde gegevens in.
Gebruik Plugin JS URL   
`https://localhost:1337/ef2gopluginhelloworld.umd.min.js`

Zet de CSP op:  
`https://localhost:1337`  

In package.json:
Zorg ervoor dat de --name op jouw plugin naam staat: `ef2goplugin<jouwpluginreference>` dus bijv. `ef2gopluginhelloworld`

## Development setup

Installeer @vue/cli globaal volgens de Vue CLI documentatie:
```
yarn global add @vue/cli
```

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn run serve
```

### Compiles and minifies for production
```
yarn run build
```



## Extra info
### Development poort veranderen:
Verander de poort (default 1337), zie package.json onderdeel scripts: serve.

`"serve": "webpack serve --static=dist --entry=./src/index.js --port=1337 --open --https --mode=development",`

### Disable Host Check
`vue.config.js`
Voor Hot Reloading (refreshing) wordt een websocket gebruikt. Aangezien jouw JS file op een andere host wordt gebruikt. Lukt het niet om een websocket verbinding op te zetten en zal hot reloading niet werken.

```
disableHostCheck: true
```

### Split chunks false
`vue.config.js`

Standaard worden dependencies in een apart js file weggeschreven.

Door de volgende instelling in de vue.config.js wordt jouw plugin als een bestand gecompiled.
```
chainWebpack: config => {
    config.optimization.splitChunks(false);
}
```
