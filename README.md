<p align="center">
  <img src="https://raw.githubusercontent.com/meilisearch/integration-guides/main/assets/logos/meilisearch_angular.svg" alt="Meilisearch-Angular" width="200" height="200" />
</p>

<h1 align="center">Meilisearch Angular</h1>

<h4 align="center">
  <a href="https://github.com/meilisearch/meilisearch">Meilisearch</a> |
  <a href="https://docs.meilisearch.com">Documentation</a> |
  <a href="https://discord.meilisearch.com">Discord</a> |
  <a href="https://roadmap.meilisearch.com/tabs/1-under-consideration">Roadmap</a> |
  <a href="https://www.meilisearch.com">Website</a> |
  <a href="https://docs.meilisearch.com/faq">FAQ</a>
</h4>

<p align="center">
  <a href="https://github.com/meilisearch/meilisearch-angular/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-MIT-informational" alt="License"></a>
</p>

<p align="center">⚡ How to integrate a front-end search bar in your Angular application using Meilisearch</p>

**Meilisearch** is an open-source search engine. [Discover what Meilisearch is!](https://github.com/meilisearch/meilisearch)

This repository describes the steps to integrate a relevant front-end search bar with a search-as-you-type experience!

## Installation

To integrate a front-end search bar, you need to install two packages:
- the open-source [Angular InstantSearch](https://github.com/algolia/angular-instantsearch/) library powered by Algolia that provides all the front-end tools you need to highly customize your search bar environment.
- the Meilisearch client [instant-meilisearch](https://github.com/meilisearch/instant-meilisearch/) to establish the communication between your Meilisearch instance and the Angular InstantSearch library.<br>
_Instead of reinventing the wheel, we have opted to reuse the InstantSearch library for our own front-end tooling. We will contribute upstream any improvements that may result from our adoption of InstantSearch._

Run:

```bash
yarn add angular-instantsearch@3.0.0-beta.5 @meilisearch/instant-meilisearch instantsearch.js
# or
npm install angular-instantsearch@3.0.0-beta.5 @meilisearch/instant-meilisearch instantsearch.js
```

NB: If you don't have any Meilisearch instance running and containing your data, you should take a look at this [getting started page](https://docs.meilisearch.com/learn/tutorials/getting_started.html).

## Getting Started

In the `app.component.ts` file, add the following code:

```js
import { Component } from '@angular/core'
import { instantMeiliSearch } from '@meilisearch/instant-meilisearch'

const searchClient = instantMeiliSearch(
  'https://demos.meilisearch.com',
  'q7QHwGiX841a509c8b05ef29e55f2d94c02c00635f729ccf097a734cbdf7961530f47c47'
)

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css'],
})
export class AppComponent {
  title = 'angular-app'
  config = {
    indexName: 'steam-video-games',
    searchClient,
  }
}

```

In the `app.module.ts` add the following code: 

```js
import { NgModule } from '@angular/core'
import { BrowserModule } from '@angular/platform-browser'
import { NgAisModule } from 'angular-instantsearch'

import { AppComponent } from './app.component'

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, NgAisModule.forRoot()],
  providers: [],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

In the `app.component.html` file, add the following code:

```html
<header class="header">
  <h1 class="header-title">Meilisearch + Angular InstantSearch</h1>
  <p class="header-subtitle">Search in Steam video games 🎮</p>

</header>

<div class="container">
  <ais-instantsearch [config]="config">
    <div class="search-panel">
      <div class="search-panel__results">
        <div class="searchbox">
          <ais-search-box placeholder=""></ais-search-box>
        </div>

        <ais-hits>
          <ng-template let-hits="hits">
            <ol class="ais-Hits-list">
              <li *ngFor="let hit of hits" class="ais-Hits-item">
                <div class="hit-name">{{hit.name}}</div>
              </li>
            </ol>
          </ng-template>
        </ais-hits>
      </div>
    </div>
  </ais-instantsearch>
</div>
```

At the bottom of `/src/polyfill.ts` file, add the following code: 
```js
;(window as any).process = {
  env: { DEBUG: undefined },
}
```

🚀 For a full getting started example, please take a look at this CodeSandbox:

[![Edit MS + Angular-IS](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/im-angularis-7xipe?file=/src/app/app.component.ts)

💡 If you have never used Angular InstantSearch before, we recommend reading this [getting started documentation](https://www.algolia.com/doc/guides/building-search-ui/what-is-instantsearch/angular/).

## Customization and Documentation

- The open-source Angular InstantSearch library is widely used and well documented in the [Algolia documentation](https://www.algolia.com/doc/api-reference/widgets/angular/). It provides all the widgets to customize and improve your search bar environment in your Angular application.
- The [instant-meilisearch documentation](https://github.com/meilisearch/instant-meilisearch/) to add some customization.
- The [Meilisearch documentation](https://docs.meilisearch.com/).

<hr>

**Meilisearch** provides and maintains many **SDKs and Integration tools** like this one. We want to provide everyone with an **amazing search experience for any kind of project**. If you want to contribute, make suggestions, or just know what's going on right now, visit us in the [integration-guides](https://github.com/meilisearch/integration-guides) repository.
