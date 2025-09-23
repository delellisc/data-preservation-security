# Golang for security
This is a repo for documenting my learning process of security practices and applying my knowledges in Go.

## Footprinting
An example of footprinting is searching for data from someone filtering by website:
```
camilo de lellis site:ifrn.edu.br
```

I found my institution ID in this [website](https://portal.ifrn.edu.br/documents/4997/Selecionados_Alimenta%C3%A7%C3%A3o.pdf). Here it is:
```
20231038060019
```

Say that I want to find a dissertation linked to my name, let's say not exactly mine, but [this file](https://saocamilo-sp.br/assets/artigo/bioethikos/78/Art12.pdf). For searching information of a specific filetype, we could use the following search term:
```
camilo de lellis dissertação filetype:pdf
```

<!-- ## Steganography

## Cryptography -->