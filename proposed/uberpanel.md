# UberPanel DRAFT


* **Editor:** JP de Vries
* **First published draft:** July 30, 2017

## Goal of Recommendation
To improve the end user experience of MODX by introducing an UberPanel component which allows packages to register widgets that display in a collapsable right sidebar. This will empower users to asynchronously accomplish more tasks within a single&nbsp;tab.

![](http://j4p.us/0O2E472B3a3L/Screen%20Shot%202017-07-30%20at%202.14.52%20AM.png)

<details open>
<summary>GIF Preview</summary>
<img src="http://j4p.us/3E2e2x2d0O39/uberpanel.gif" />
</details>


## Relevant Recommendations
n/a

## Proofs of Concept

 - [UberPanel CodePen](https://codepen.io/jpdevries/pen/XamPKV)

## Recommendation

The MODX 2.x user experience requires frequently navigating from manager page to manager to accomplish various tasks. In a future manager architected with modern standards, the MODX Advisory Board recommends core distributions ship with an API to register panels to a collapsible right sidebar.

With a straightforward JavaScript API, MODX Extras can register widgets to appear in the UberPanel. This allows for features like Live Preview and Media Management to enhance the MODX experience.

Like the MODX tree, the UberPanel is collapsible. This allows users to keep panels in or out of view as necessary.

UberPanels should integrate with Access Control Lists so that which user groups see which panels can be configurable.

The MODX core:
 - provides an API to register panels
 - renders interface components to show and collapse the&nbsp;`UberPanel`
 - displays a `<select>` of available panels to quickly navigate between them
 - dispatches events for responding to panel transition state

## Example PHP API

A PHP API would register panels and output them in the initial HTML layer of MODX Manager pages, making panels available for `.no-js`&nbsp;experiences.

```php
<?php
/* or whatever */
$modx->uber->panel.addItem('getResources', $modx->getOption('assets_url') . 'mycomponent/panel/getResources.php', 'getResources');
```

## Example JavaScript API

*Below is an example of how an UberPanel API could be created. This Recommendation only offers this as an example.*

### `<iframe>` usage
```js
// create an iFrame
const iframe = document.createElement('iframe');
// set the iFrame source
iframe.src = 'https://modx.com';
// set the unique package name
iframe.dataset.panel = 'modx.com';
// optionally set the display name
iframe.dataset.displayName = 'MODX Homepage';

// finally, register the panel
MODX.uber.panel.addItem('getResources', iframe);
```

### Web Component Usage

Classical Usage
```js
lazyLoadScript(path.join(MODX.assetsURL, 'components/mycomponent/js/app.js'), 'mycomponent').then(() => {
  // create your component, which should extend HTMLElement
  const myComponent = new MyComponent();

  // set the unique package name
  component.dataset.panel = 'mycomponent';

  // optionally set the display name
  component.dataset.displayName = 'My Component';

  // finally, register the panel
  MODX.uber.panel.addItem('mycomponent', myComponent);
})
```

Prototypal Usage
```js
lazyLoadScript(path.join(MODX.assetsURL, 'components/mycomponent/js/app.js'), 'mycomponent').then(() => {
  // create your component, which should return an HTMLElement
  const myComponent = MyComponent();

  // set the unique package name
  component.dataset.panel = 'mycomponent';

  // optionally set the display name
  component.dataset.displayName = 'My Component';

  // finally, register the panel
  MODX.uber.panel.addItem('mycomponent', myComponent);
})
```

### Shadow DOM Usage

WIP

### Listening to Panel Events

```js
// create an iFrame
const iframe = document.createElement('iframe');
// set the iFrame source
iframe.src = 'https://modx.com';
// set the unique package name
iframe.dataset.panel = 'modx.com';
// optionally set the display name
iframe.dataset.displayName = 'MODX Homepage';

// finally, register the panel
const panelItem = MODX.uber.panel.addItem('getResources', iframe);

panelItem.addEventListener(MODX.uber.panel.TRANSITION_IN, (event) => {
  // panelItem is transitioning into view
  const panelItem = event.target;
});

panelItem.addEventListener(MODX.uber.panel.TRANSITION_OUT, (event) => {
  // panelItem is transitioning out of view
  const panelItem = event.target;
});
```
