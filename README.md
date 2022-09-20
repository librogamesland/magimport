# Magimport

Tool per importare file, trasformandoli in file compatibili con magebook. Si tratta di uno strumento avanzato, volutamente poco user-friendly ma molto versatile e potente. **DEMO ANCORA IN VIA DI SVILUPPO**

## Come iniziare
Apri Magimport <https://librogamesland/magimport>. Sulla sinistra trovi uno spazio per copiaincollare il libro, mentre in basso c'è uno spazio in cui inserire lo script che trasformi il contenuto copiaincollato in un file di magebook. A destra comparirà il risultato.

Nel caso di file pdf, conviene passare attraverso Google Documents, caricando il file su google drive, convertendolo, scaricandolo in formato HTML e poi copiaincollando dal file HTML.


## Script
Ogni volta che si clicca su `run` o si preme `ctrl+r`, il programma provvedererà a creare una copia del libro nell'area a destra (che ha id = `#result`), ed eseguirà quindi lo script. 

La strategia più efficace consiste nel dividere il lavoro in due fasi: 
- una prima fase in cui si lavora sull'HTML, "marchiando" le formattazioni di interesse. È possibile analizzare la struttura dell'HTML copiato tramite lo strumento "ispeziona" incluso nei browser.
- una seconda fase in cui si eliminano tutte le formattazioni esistenti usando la funzione innerText e si procede lavorando sul testo puro

In questo modo si trae vantaggio da tutte le informazioni presenti nel testo copiaincollato. Ad esempio, se il testo presenta già dei link, questi probabilmente avranno tutti la stessa formattazione (ad esempio saranno traslati con il tag html `<a href="numero">titolo</a>`), ed è possibile individuarli e aggiungerci le parentesi quadre con:
```js
document.querySelectorAll('#result a[href]').forEach( 
    el => el.textContent = '[' + el.textContent + ']'
)
```

Nella seconda fase, invece, si può intervenire più facilmente su elementi che coinvolgono più righe con strumenti come le regular expression. Ad esempio, si può imporre che i titoli abbiano sempre due righe sopra e nessuna sotto con:
```js
result.innerText = result.innerText.replaceAll(/[\s\n]+(### [0-9]+\**)[\s\n]+/gm, '\n\n\n$1\n')
```

In futuro verranno aggiunti esempi di strategie più avanzate, come l'uso di DOM TreeWalker o di liste di frasi che precedono i collegamenti (come fa LibrogameCreator).


## Esempi

Conversione di un sito html generato da magebook:
```js
document.querySelectorAll('#result h2').forEach( 
    el => el.innerHTML = '### ' + el.textContent.trim()
)
document.querySelectorAll('#result i').forEach( 
    el => el.textContent = '<i>' + el.textContent + '</i>'
)
document.querySelectorAll('#result b').forEach( 
    el => el.textContent = '<b>' + el.textContent + '</b>'
)
document.querySelectorAll('#result u').forEach( 
    el => el.textContent = '<u>' + el.textContent + '</u>'
)
document.querySelectorAll('#result a').forEach( 
    el => el.textContent = '[' + el.textContent + ']'
)

result.innerText = result.innerText.replaceAll('\n\n', '\n').split('\n').map(
    line => line.trim()).join('\n')
```


File libreoffice, creato a partire da magebook (ma in generale i libreoffice); i numeri di paragrafo erano sulla stessa linea con una parentesi) dopo:
```js

document.querySelectorAll('#result a[name] ~ font b').forEach( 
    el => el.outerHTML = '### ' + el.textContent.trim().replaceAll(')', '') + '<br>'
)

document.querySelectorAll('#result i').forEach( 
    el => el.textContent = '<i>' + el.textContent + '</i>'
)

document.querySelectorAll('#result b').forEach( 
    el => el.textContent = '<b>' + el.textContent + '</b>'
)

document.querySelectorAll('#result u').forEach( 
    el => el.textContent = '<u>' + el.textContent + '</u>'
)

document.querySelectorAll('#result a[href]').forEach( 
    el => el.textContent =  el.textContent
)

result.innerText = result.innerText.replaceAll('\n\n', '\n')
    .replaceAll('</i><i>', '').replaceAll('</b><b>', '').replaceAll('</u><u>', '')
    .replaceAll('\n###\n', '\n')
    .split('\n').map(
        line => line.trim()
    ).join('\n')

```




Alla luce del buio (doc, paragrafi numerici con molti segnalibri mancanti):
```js
document.querySelectorAll('#result a[href]').forEach( 
    el => el.textContent = '[' + el.textContent + ']'
)
document.querySelectorAll('#result i').forEach( 
    el => el.textContent = '<i>' + el.textContent + '</i>'
)
document.querySelectorAll('#result a[name] ~ font b').forEach( 
    el => el.outerHTML = '### ' + el.textContent.trim().replaceAll(')', '') + '<br>'
)
document.querySelectorAll('#result b').forEach( 
    el => el.textContent = '<b>' + el.textContent + '</b>'
)
document.querySelectorAll('#result u').forEach( 
    el => el.textContent = '<u>' + el.textContent + '</u>'
)

result.innerText = result.innerText.replaceAll('\n\n', '\n').replaceAll('\n\n\n', '\n\n')
    .replaceAll(/\n<b>([0-9]*\*+)<\/b>/g, '\n### $1')
    .replaceAll(/[\s\n]+(### [0-9]+\**)[\s\n]+/gm, '\n\n\n$1\n')
    .replaceAll('</i><i>', '').replaceAll('</b><b>', '').replaceAll('</u><u>', '')
    .replaceAll('\n###\n', '\n')
    .split('\n').map(
        line => line.trim()
    ).join('\n')
```



## Licenza e autori

Autore: Luca Fabbian <luca.fabbian.1999@gmail.com>

Il codice di Magimport è distribuito sotto licenza libera MIT. All'interno della repo, per comodità, è incluso Ace editor.