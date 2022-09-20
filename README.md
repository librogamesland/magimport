

Siti html generati da magebook
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


File libreoffice, creato a partire da magebook:
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