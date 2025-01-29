# Disallow calling `pipe` within a `pipe` callback (`rxjs-x/no-nested-pipe`)

💭 This rule requires [type information](https://typescript-eslint.io/linting/typed-linting).

<!-- end auto-generated rule header -->

This rule effects failures if `pipe` is called within a `pipe` handler.

## Rule details

Examples of **incorrect** code for this rule:

```ts
import { switchMap, map, of } from 'rxjs';

of('searchText1', 'searchText2')
  .pipe(
    switchMap(searchText => {
      return getSearchResult(searchText);
    })
  )
  .subscribe(value => console.log(value));

function getSearchResult(searchText) {
  return of('new' + searchText).pipe(
    map(response => {
      console.log(response);
      return 'final ' + response;
      // considering more lines here
    })
  );
}
```

Examples of **correct** code for this rule:

```ts
import { switchMap, map, of } from 'rxjs';

of('searchText1', 'searchText2')
  .pipe(
    switchMap(searchText => {
      return getSearchResult(searchText).pipe(
        map(response => {
          console.log(response);
          return 'final ' + response;
          // considering more lines here
        })
      );
    })
  )
  .subscribe(value => console.log(value));

function getSearchResult(searchText) {
  return of('new' + searchText);
}
```

## Options

This rule has no options.
