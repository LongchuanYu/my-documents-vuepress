---
title: 页面内多个按钮spinner
date: 2021-10-31 13:32:16
permalink: /pages/a28b2d/
categories:
  - Angular
tags:
  - 
---
## angular实现页面内多个按钮spinner

#### ts
```ts
import { Component } from '@angular/core';

@Component({
  selector: 'my-app',
  templateUrl: './app.component.html',
  styleUrls: [ './app.component.css' ]
})
export class AppComponent  {
  name = 'Angular 5';
  button = 'Submit';
  isLoading = false;
  buttons = {
    button1: {
      name: 'Button 1',
      loading: false
    },
    button2: {
      name: 'Button 2',
      loading: false
    },
    button3: {
      name: 'Button 3',
      loading: false
    },
  }

  constructor() {

  }

  click() {
    this.isLoading = true;
    this.button = 'Processing';

    setTimeout(() => {
      this.isLoading = false;
      this.button = 'Submit';
      alert('Done loading');
    }, 8000)
  }

  clickMulti(name: string) {
    this.buttons[name]['name'] = 'Processing';
    this.buttons[name]['loading'] = true;
    setTimeout(() => {
      this.buttons[name]['name'] = 'Submit';
    this.buttons[name]['loading'] = false;
      alert('Done loading' + name);
    }, 8000)
  }
}

```

#### html
```html
<hello name="{{ name }}"></hello>
<p>
  Start editing to see some magic happen :)
</p>

<p>One button</p>
<button class="btn btn-primary" [disabled]="isLoading" (click)="click()"><i class="fa" [ngClass]="{'fa-spin fa-spinner': isLoading, 'fa-check': !isLoading}"></i> {{button}}</button>
<hr>
<p>Multiple button</p>
<button class="btn btn-primary" [disabled]="buttons['button1']['loading']" (click)="clickMulti('button1')"><i class="fa" [ngClass]="{'fa-spin fa-spinner': buttons['button1']['loading'], 'fa-check': !buttons['button1']['loading']}"></i> {{buttons['button1']['name']}}</button>
<button class="btn btn-primary" [disabled]="buttons['button2']['loading']" (click)="clickMulti('button2')"><i class="fa" [ngClass]="{'fa-spin fa-spinner': buttons['button2']['loading'], 'fa-check': !buttons['button2']['loading']}"></i> {{buttons['button2']['name']}}</button>
<button class="btn btn-primary" [disabled]="buttons['button3']['loading']" (click)="clickMulti('button3')"><i class="fa" [ngClass]="{'fa-spin fa-spinner': buttons['button3']['loading'], 'fa-check': !buttons['button3']['loading']}"></i> {{buttons['button3']['name']}}</button>
```