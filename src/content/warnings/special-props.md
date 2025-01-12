---
title: Предупреждение о специальных реквизитах
---

Большинство реквизитов элемента JSX передаются компоненту, однако есть два специальных реквизита (`ref` и `key`), которые используются React и, таким образом, не пересылаются компоненту.

Например, вы не можете прочитать `props.key` из компонента. Если вам нужно получить доступ к тому же значению внутри дочернего компонента, вы должны передать его как другой реквизит (например: `<ListItemWrapper key={result.id } id={result.id } />` и прочитать `props.id `). Хотя это может показаться излишним, важно отделять логику приложения от подсказок для реагирования.
