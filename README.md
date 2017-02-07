# angular2-froala-wyswiyg
>angular2-froala-wyswiyg provides Angular2 bindings to the Froala WYSIWYG editor VERSION 2.

## Table of contents 
2. [Installation instructions](#installation-instructions)
3. [Integration](#integration)
4. [Usage](#usage)
5. [Manual Initialization](#manual-initialization)
6. [Displaying HTML](#displaying-html)
7. [License](#license)

## Installation instructions

Install `angular2-froala-wysiwyg` from `npm`

```bash
npm install angular2-froala-wysiwyg --save
```

You will need CSS styles and Font Awesome

```html
<!-- index.html -->
<link href="node_modules/froala-editor/css/froala_editor.pkgd.min.css" rel="stylesheet">
<link href="node_modules/font-awesome/css/font-awesome.min.css" rel="stylesheet">
```



## Integration

### Use with `angular-cli`

#### Installing angular-cli

*Important*: please check `angular-cli` version, it should be => "1.0.0-beta.24".

*Note*: you can skip this part if you already have application generated by `ng-cli` and webpack.

```bash
npm i -g angular-cli
ng new my-app
cd my-app
```

#### Adding angular2-froala-wysiwyg

- install `angular2-froala-wysiwyg`

```bash
npm install angular2-froala-wysiwyg --save
```

- open `src/app/app.module.ts` and add

```typescript
# Import editor JS.
import "froala-editor/js/froala_editor.pkgd.min.js";

# Import Angular2 plugin.
import { FroalaEditorModule } from 'angular2-froala-wysiwyg';
...

@NgModule({
   ...
   imports: [FroalaEditorModule.forRoot(), FroalaViewModule.forRoot() ... ],
   ... 
})
```

- open `angular-cli.json` and insert a new entry into the styles array 

```json
"styles": [
  "styles.css",
  "../node_modules/font-awesome/css/font-awesome.css",
  "../node_modules/froala-editor/css/froala_editor.pkgd.min.css"
],
```

- in `angular-cli.json` add the following to load Font Awesome fonts

```json
"addons": [
	"../node_modules/font-awesome/fonts/*.+(otf|eot|svg|ttf|woff|woff2)"
],
```

- open `src/app/app.component.html` and add
```html
<div [froalaEditor]>Hello, Froala!</div>
```

#### Run angular-cli
```bash
ng serve
```



### Use with `webpack`

#### Create webpack app

*Note*: you can skip this part if you already have application generated.

```bash
git clone --depth 1 https://github.com/angularclass/angular2-webpack-starter.git
cd angular2-webpack-starter
npm install
```

#### Adding angular2-froala-wysiwyg

- install `angular2-froala-wysiwyg`

```bash
npm install angular2-froala-wysiwyg --save
```

- open `src/app/app.module.ts` and add

```typescript
# Import editor JS.
import "froala-editor/js/froala_editor.pkgd.min.js";

# Import Angular2 plugin.
import { FroalaEditorModule } from 'angular2-froala-wysiwyg';
...

@NgModule({
   ...
   imports: [FroalaEditorModule.forRoot(), FroalaViewModule.forRoot(), ... ],
   ... 
})
```

- open `src/app/app.component.ts` and add to the template

```html
<div [froalaEditor]>Hello, Froala!</div>
```

- open `config/webpack.common.js` and add the following to `CopyWebpackPlugin`

```javascript
{
  from: 'node_modules/froala-editor/css/',
  to: 'assets/froala-editor/css/',
},
{
  from: 'node_modules/font-awesome/css/font-awesome.min.css',
  to: 'assets/font-awesome/css/font-awesome.min.css',
},
{
  from: 'node_modules/font-awesome/fonts',
  to: 'assets/font-awesome/fonts'
}
```

- open `config/head-config.common.js` and add a new entry to link

```javascript
{ rel: 'stylesheet', href: '/assets/font-awesome/css/font-awesome.min.css' },
{ rel: 'stylesheet', href: '/assets/froala-editor/css/froala_editor.pkgd.min.css' }
```

#### Run webpack app

```bash
npm run start
```



## Usage

### Options

You can pass editor options as Input (optional).

`[froalaEditor]='options'`

You can pass any existing Froala option. Consult the [Froala documentation](https://www.froala.com/wysiwyg-editor/docs/options) to view the list of all the available options:

```typescript
public options: Object = { 
  placeholderText: 'Edit Your Content Here!',
  charCounterCount: false
}
```

Aditional option is used:
* **immediateAngularModelUpdate**: (default: false) This option synchronizes the angular model as soon as a key is released in the editor. Note that it may affect performances.



### Events and Methods

Events can be passed in with the options, with a key events and object where the key is the event name and the value is the callback function.

```typescript
public options: Object = {
  placeholder: "Edit Me",
  events : {
    'froalaEditor.focus' : function(e, editor) {
      console.log(editor.selection.get());
    }
  }
}
```

Using the editor instance from the arguments of the callback you can call editor methods as described in the [method docs](http://froala.com/wysiwyg-editor/docs/methods).

Froala events are described in the [events docs](https://froala.com/wysiwyg-editor/docs/events).



### Model

The WYSIWYG HTML editor content model.

`[(froalaModel)]="editorContent"`

Pass initial content:

```typescript
public editorContent: string = 'My Document\'s Title'
```

Use the content in other places:

```html
<input [ngModel]="editorContent"/>
<input [(ngModel)]="editorContent"/> <!-- For two way binding -->
```

Other two way binding example:

```html
<div [froalaEditor] [(froalaModel)]="editorContent"></div>
<div [froalaEditor] [(froalaModel)]="editorContent"></div>
```



### Special tags

You can also use the editor on **img**, **button**, **input** and **a** tags:

```html
<img [froalaEditor] [(froalaModel)]="imgObj"/>
```

The model must be an object containing the attributes for your special tags. Example:

```typescript
public imgObj: Object = {
  src: 'path/to/image.jpg'
};
```

The froalaModel will change as the attributes change during usage.

* froalaModel can contain a special attribute named **innerHTML** which inserts innerHTML in the element: If you are using 'button' tag, you can specify the button text like this:

```typescript
public buttonModel: Object = {
  innerHTML: 'Click Me'
};
```
As the button text is modified by the editor, the **innerHTML** attribute from buttonModel model will be modified too.



### Specific option for special tags

* **angularIgnoreAttrs**: (default: null) This option is an array of attributes that you want to ignore when the editor updates the froalaModel:

```typescript
public inputOptions: Object = {
  angularIgnoreAttrs: ['class', 'ng-model', 'id']
};
```



## Manual Initialization

Gets the functionality to operate on the editor: create, destroy and get editor instance. Use it if you want to manually initialize the editor.

`(froalaInit)="initialize($event)"`

Where `initialize` is the name of a function in your component which will receive an object with different methods to control the editor initialization process.

```typescript
public initialize(initControls) {
  this.initControls = initControls;
  this.deleteAll = function() {
  	this.initControls.getEditor()('html.set', '');
  };
}
```

The object received by the function will contain the following methods:

- **initialize**: Call this method to initialize the Froala Editor
- **destroy**: Call this method to destroy the Froala Editor
- **getEditor**: Call this method to retrieve the editor that was created. This method will return *null* if the editor was not yet created




## Displaying HTML

To display content created with the froala editor use the froalaView directive.

`[froalaView]="editorContent"`

```html
<div [froalaEditor] [(froalaModel)]="editorContent"></div>
<div [froalaView]="editorContent"></div>
```



## License

The `angular2-froala-wyswiyg` project is under MIT license. However, in order to use Froala WYSIWYG HTML Editor plugin you should purchase a license for it.

Froala Editor has [3 different licenses](http://froala.com/wysiwyg-editor/pricing) for commercial use.
For details please see [License Agreement](http://froala.com/wysiwyg-editor/terms).
