---
title: Предупреждение о неизвестном реквизите
---

Предупреждение unknown-prop сработает, если вы попытаетесь отобразить элемент DOM с prop, который не распознается React как допустимый атрибут/свойство DOM. Вы должны убедиться, что в ваших элементах DOM нет поддельных реквизитов, плавающих вокруг.

Есть несколько вероятных причин, по которым может появиться это предупреждение:

1. Используете ли вы `{...props}` или `cloneElement(element, props)`? При копировании реквизитов в дочерний компонент вы должны убедиться, что вы случайно не пересылаете реквизиты, которые предназначались только для родительского компонента. Смотрите общие исправления этой проблемы ниже.

2. Вы используете нестандартный атрибут DOM на собственном узле DOM, возможно, для представления пользовательских данных. Если вы пытаетесь прикрепить пользовательские данные к стандартному элементу DOM, рассмотрите возможность использования пользовательского атрибута данных, как описано [on MDN](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Using_data_attributes).

3. React пока не распознает указанный вами атрибут. Вероятно, это будет исправлено в будущей версии React. React позволит вам передать его без предупреждения, если вы введете имя атрибута в нижнем регистре.

4. Вы используете компонент React без верхнего регистра, например `<MyButton />`. React интерпретирует его как тег DOM, потому что React JSX transform использует соглашение о верхнем и нижнем регистре для различения пользовательских компонентов и тегов DOM. Для ваших собственных компонентов React используйте PascalCase. Например, напишите `<MyButton />` вместо `<MyButton />`.
5. 
---

Если вы получаете это предупреждение из-за того, что передаете реквизиты типа `{...props}`, ваш родительский компонент должен "использовать" любой реквизит, который предназначен для родительского компонента и не предназначен для дочернего компонента. Пример:
**Ошибка:** Неожиданный реквизит 'layout' перенаправляется в теге 'div'.

```js
function MyDiv(props) {
  if (props.layout === 'horizontal') {
    // ПЛОХО! Потому что вы точно знаете, что "макет" - это не реквизит, который <div> понимает.
    return <div {...props} style={getHorizontalStyle()} />
  } else {
    // ПЛОХО! Потому что вы точно знаете, что "макет" - это не реквизит, который <div> понимает.
    return <div {...props} style={getVerticalStyle()} />
  }
}
```

**Правильно:** Синтаксис spread можно использовать для извлечения переменных из props и помещения оставшихся props в переменную.

```js
function MyDiv(props) {
  const { layout, ...rest } = props
  if (layout === 'horizontal') {
    return <div {...rest} style={getHorizontalStyle()} />
  } else {
    return <div {...rest} style={getVerticalStyle()} />
  }
}
```

**Правильно:** Вы также можете назначить реквизиты новому объекту и удалить ключи, которые вы используете, из нового объекта. Обязательно не удаляйте реквизит из исходного объекта `this.props`, поскольку этот объект следует считать неизменяемым.

```js
function MyDiv(props) {
  const divProps = Object.assign({}, props);
  delete divProps.layout;

  if (props.layout === 'horizontal') {
    return <div {...divProps} style={getHorizontalStyle()} />
  } else {
    return <div {...divProps} style={getVerticalStyle()} />
  }
}
```