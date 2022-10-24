---
layout: single
title: 이유정 복습일지
categories: 복습일지 중 하나
tags: 이유정
---

```js
const root = document.getElementById('root');
function createTreeView(menu, currentNode) {
  menu.map(el=> {
    const liElement = document.createElement('li')
    currentNode.append(liElement)
    if(el.children){
      const inputElement = document.createElement('input')
      inputElement.setAttribute("type", "checkbox")
      liElement.append(inputElement)
      const spanElement = document.createElement('span')
      spanElement.textContent = el.name
      liElement.append(spanElement)
      const ulElement = document.createElement('ul')
      liElement.append(ulElement)
      createTreeView(el.children, ulElement)
    }
     else if(!el.children){
        liElement.textContent = el.name
      }
  })
}

createTreeView(menu, root);
```
